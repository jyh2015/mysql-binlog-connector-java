# mysql-binlog-connector-java

MySQL Binary Log connector.

Initially project was started as a fork of [open-replicator](https://code.google.com/p/open-replicator), but ended up as a complete rewrite. Key differences (features):

- automatic binlog filename/position resolution
- resumable disconnects
- plugable failover strategies
- JMX exposure (optionally with statistics)
- availability in Maven Central (deferred until everything is thoroughly tested)
- no third-party dependencies
- binlog_checksum support (for MySQL 5.6.2+ users)
- test suite over different versions of MySQL releases

## Usage

The latest development version always available through Sonatype Snapshots repository (see example below).

```xml
<dependencies>
    <dependency>
        <groupId>com.github.shyiko</groupId>
        <artifactId>mysql-binlog-connector-java</artifactId>
        <version>0.1.0-SNAPSHOT</version>
    </dependency>
</dependencies>

<repositories>
    <repository>
    <id>sonatype-snapshots</id>
    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
    <snapshots>
        <enabled>true</enabled>
    </snapshots>
    <releases>
        <enabled>false</enabled>
    </releases>
    </repository>
</repositories>
```

### Reading Binary Log file

    File binlogFile = ...
    BinaryLogFileReader reader = new BinaryLogFileReader(binlogFile);
    try {
        for (Event event; (event = reader.readEvent()) != null; ) {
            ...
        }
    } finally {
        reader.close();
    }

### Tapping into MySQL replication stream

    BinaryLogClient client = new BinaryLogClient("hostname", 3306, "username", "password")
    client.registerEventListener(new EventListener() {

        @Override
        public void onEvent(Event event) {
            ...
        }
    });
    client.connect();

## Documentation

For the insight into the internals of MySQL look [here](https://dev.mysql.com/doc/internals/en/index.html). [MySQL Client/Server Protocol](http://dev.mysql.com/doc/internals/en/client-server-protocol.html) and [The Binary Log](http://dev.mysql.com/doc/internals/en/binary-log.html) sections are particularly useful as a reference documentation for the `com.**.mysql.binlog.network` and `com.**.mysql.binlog.event` packages.

## License

[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)
