---
title: "Log4j2模板"
subtitle: ""
date: '2025-05-17T03:09:00+08:00'
lastmod: '2026-01-27T11:01:00+08:00'
draft: false
categories: ['模板']
tags: ['Log4j2','xml','yaml']
---



> log4j2 pattern format [website](https://logging.apache.org/log4j/2.x/manual/pattern-layout.html)

#### config xml file

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN" monitorInterval="15">
    <properties>
        <property name="LOG_HOME">logs</property>
        <property name="PATTERN_FORMAT">%d{HH:mm:ss.SSS} [%t] [%-5level] %c.%M(%L) - %msg%n</property>
    </properties>

    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="${PATTERN_FORMAT}" />
            <ThresholdFilter level="DEBUG" onMatch="ACCEPT" onMismatch="NEUTRAL" />
        </Console>

        <RollingRandomAccessFile name="DebugFile"
                                 fileName="${LOG_HOME}/debug.log"
                                 filePattern="${LOG_HOME}/$${date:yyyy-MM-dd}/debug-%d{yyyy-MM-dd}-%i.log.zip">
            <Filters>
                <ThresholdFilter level="INFO" onMatch="DENY" onMismatch="NEUTRAL" />
                <ThresholdFilter level="DEBUG" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
            <PatternLayout pattern="${PATTERN_FORMAT}" />
            <Policies>
                <TimeBasedTriggeringPolicy />
                <SizeBasedTriggeringPolicy size="15 MB" />
            </Policies>
            <DefaultRolloverStrategy max="20" >
                <Delete basePath="${LOG_HOME}/" maxDepth="5">
                    <IfFileName glob="*debug*.log.zip"/>
                </Delete>
            </DefaultRolloverStrategy>
        </RollingRandomAccessFile>

        <RollingRandomAccessFile name="InfoFile"
                                 fileName="${LOG_HOME}/info.log"
                                 filePattern="${LOG_HOME}/$${date:yyyy-MM-dd}/info-%d{yyyy-MM-dd}-%i.log.zip">
            <Filters>
                <ThresholdFilter level="WARN" onMatch="DENY" onMismatch="NEUTRAL" />
                <ThresholdFilter level="INFO" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
            <PatternLayout pattern="${PATTERN_FORMAT}" />
            <Policies>
                <TimeBasedTriggeringPolicy />
                <SizeBasedTriggeringPolicy size="15 MB" />
            </Policies>
            <DefaultRolloverStrategy max="20" >
                <Delete basePath="${LOG_HOME}/" maxDepth="5">
                    <IfFileName glob="*info*.log.zip"/>
                </Delete>
            </DefaultRolloverStrategy>
        </RollingRandomAccessFile>

        <RollingRandomAccessFile name="ErrorFile"
                                 fileName="${LOG_HOME}/error.log"
                                 filePattern="${LOG_HOME}/$${date:yyyy-MM-dd}/error-%d{yyyy-MM-dd}-%i.log.zip">
            <Filters>
                <ThresholdFilter level="FATAL" onMatch="DENY" onMismatch="NEUTRAL" />
                <ThresholdFilter level="WARN" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
            <PatternLayout pattern="${PATTERN_FORMAT}" />
            <Policies>
                <TimeBasedTriggeringPolicy />
                <SizeBasedTriggeringPolicy size="10 MB" />
            </Policies>
            <DefaultRolloverStrategy max="15" >
                <Delete basePath="${LOG_HOME}/" maxDepth="5">
                    <IfFileName glob="*error*.log.zip"/>
                </Delete>
            </DefaultRolloverStrategy>
        </RollingRandomAccessFile>

        <RollingRandomAccessFile name="FatalFile"
                                 fileName="${LOG_HOME}/fatal.log"
                                 filePattern="${LOG_HOME}/$${date:yyyy-MM}/fatal-%d{yyyy-MM-dd}-%i.log.zip">
            <Filters>
                <ThresholdFilter level="FATAL" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
            <PatternLayout pattern="${PATTERN_FORMAT}" />
            <Policies>
                <TimeBasedTriggeringPolicy />
                <SizeBasedTriggeringPolicy size="5 MB" />
            </Policies>
            <DefaultRolloverStrategy max="5" >
                <Delete basePath="${LOG_HOME}/" maxDepth="5">
                    <IfFileName glob="*fatal*.log.zip"/>
                </Delete>
            </DefaultRolloverStrategy>
        </RollingRandomAccessFile>
    </Appenders>

    <Loggers>
        <Logger name="org.apache" level="ERROR"/>
        <Logger name="org.mybatis" level="DEBUG"/>

        <Root level="INFO">
            <AppenderRef ref="Console" />
            <AppenderRef ref="DebugFile"/>
            <AppenderRef ref="InfoFile" />
            <AppenderRef ref="ErrorFile" />
            <AppenderRef ref="FatalFile" />
        </Root>
    </Loggers>
</Configuration>
```

#### config yaml file

```yaml
Configuration:
  status: warn
  Properties:
    Property:
      - name: LOG_HOME
        value: logs
      - name: PATTERN_FORMAT
        value: "%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] [%-5level] %c{1.5.*}.%M(%L) - %msg%n%throwable"
  Appenders:
    Console:
      name: ConsoleAppender
      target: SYSTEM_OUT
      PatternLayout:
        pattern: ${PATTERN_FORMAT}
    RollingRandomAccessFile:
      name: FileAppender
      fileName: ${LOG_HOME}/app.log
      filePattern: "${LOG_HOME}/%d{yyyy-MM}-week-${date:w}/%d{yyyy-MM-dd}-%i.log.zip"
      PatternLayout:
        pattern: ${PATTERN_FORMAT}
      Policies:
        SizeBasedTriggeringPolicy:
          size: 50MB
        TimeBasedTriggeringPolicy:
          interval: 1
          modulate: true
      DefaultRolloverStrategy:
        max: 30
        Delete:
          basePath: ${LOG_HOME}
          maxDepth: 3
          IfFileName:
            glob: "*/week-*/*.log.zip"

  Loggers:
    Root:
      level: info
      AppenderRef:
        - ref: ConsoleAppender
        - ref: FileAppender
    Logger:
      - name: org.apache
        level: warn
      - name: org.mybatis
        level: debug
```

#### springboot指定yaml配置文件
```yaml
logging:
  config: classpath:log4j2.yaml
```