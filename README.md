# jetcd - A Java Client for etcd
[![Build Status](https://github.com/etcd-io/jetcd/actions/workflows/build-master.yml/badge.svg)](https://github.com/etcd-io/jetcd/actions)
[![License](https://img.shields.io/badge/Licence-Apache%202.0-blue.svg?style=flat-square)](http://www.apache.org/licenses/LICENSE-2.0.html)
[![Maven Central](https://img.shields.io/maven-central/v/io.etcd/jetcd-core.svg?style=flat-square)](https://search.maven.org/#search%7Cga%7C1%7Cio.etcd)
[![GitHub release](https://img.shields.io/github/release/etcd-io/jetcd.svg?style=flat-square)](https://github.com/etcd-io/jetcd/releases)
[![Javadocs](http://www.javadoc.io/badge/io/etcd/jetcd-core.svg)](https://javadoc.io/doc/io.etcd/jetcd-core)

jetcd is the official java client for [etcd](https://github.com/etcd-io/etcd) v3.

> Note: jetcd is work-in-progress and may break backward compatibility.

## Java Versions

Java 8 or above is required.

## Download

### Maven
```xml
<dependency>
  <groupId>io.etcd</groupId>
  <artifactId>jetcd-core</artifactId>
  <version>${jetcd-version}</version>
</dependency>
```

Development snapshots are available in [Sonatypes's snapshot repository](https://oss.sonatype.org/content/repositories/snapshots/io/etcd).

### Gradle

```
dependencies {
    compile "io.etcd:jetcd-core:$jetcd-version"
}
```

### Usage

```java
// create client
Client client = Client.builder().endpoints("http://localhost:2379").build();
KV kvClient = client.getKVClient();

ByteSequence key = ByteSequence.from("test_key".getBytes());
ByteSequence value = ByteSequence.from("test_value".getBytes());

// put the key-value
kvClient.put(key, value).get();

// get the CompletableFuture
CompletableFuture<GetResponse> getFuture = kvClient.get(key);

// get the value from CompletableFuture
GetResponse response = getFuture.get();

// delete the key
kvClient.delete(key).get();
```

For full etcd v3 API, plesase refer to the [official API documentation](https://etcd.io/docs/current/learning/api/).

### Examples

The [examples](https://github.com/etcd-io/jetcd/tree/master/jetcd-examples) are standalone projects that show usage of jetcd.

## Launcher

The `io.etcd:jetcd-test` offers a convenient utility to programmatically start & stop an isolated `etcd` server.  This can be very useful e.g. for integration testing, like so:

```java
import io.etcd.jetcd.Client;
import io.etcd.jetcd.launcher.EtcdCluster;
import io.etcd.jetcd.test.EtcdClusterExtension;
import org.junit.jupiter.api.extension.RegisterExtension;

@RegisterExtension static final EtcdCluster etcd = new EtcdClusterExtension("test-etcd", 1);
Client client = Client.builder().endpoints(etcd.getClientEndpoints()).build();
```

This launcher uses the Testcontainers framework.
For more info and prerequisites visit [testcontainers.org](https://www.testcontainers.org).

## Versioning

The project follows [Semantic Versioning](http://semver.org/).

The current major version is zero (0.y.z). Anything may change at any time. The public API should not be considered stable.

## Build from source

The project can be built with [Apache Maven](https://maven.apache.org/):

```
./gradlew compileJava
```

## Running tests

The project is tested against a three node `etcd` setup started with the Launcher (above) :

```sh
$ ./gradlew test
````

### Troubleshooting

It recommmonds building the project before running tests so that you have artifacts locally. It will solve some problems if the latest snapshot hasn't been uploaded or network issues.

## Contact

* Mailing list: [etcd-dev](https://groups.google.com/forum/?hl=en#!forum/etcd-dev)

## Contributing

See [CONTRIBUTING](https://github.com/etcd-io/jetcd/blob/master/CONTRIBUTING.md) for details on submitting patches and the contribution workflow.

## Reporting bugs

See [reporting bugs](https://github.com/etcd-io/etcd/blob/master/Documentation/reporting-bugs.md) for details about reporting any issues.

## License

jetcd is under the Apache 2.0 license. See the [LICENSE](https://github.com/etcd-io/jetcd/blob/master/LICENSE) file for details.
