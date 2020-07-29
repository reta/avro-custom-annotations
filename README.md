Using Apache Avro with custom converters and Java annotations
==============

- Build: 
    
        mvn clean package

- Apache Avro Maven schema

        {
            "type": "record",
            "name": "PaymentRejectedEvent",
            "namespace": "com.example.event",
            "javaAnnotation": "javax.annotation.Generated(\"avro\")",
            "fields": [
                {
                    "name": "id",
                    "type": {
                        "type": "string",
                        "logicalType": "uuid"
                    }
                },
                {
                    "name": "reason",
                    "type": {
                        "type": "enum",
                        "name": "PaymentStatus",
                        "namespace": "com.example.event",
                        "symbols": [
                            "EXPIRED_CARD",
                            "INSUFICIENT_FUNDS",
                            "DECLINED"
                        ]
                    }
                },
                {
                    "name": "date",
                    "type": {
                        "type": "long",
                        "logicalType": "local-timestamp-millis"
                    }
                }
            ]
        }

- Apache Avro Maven plugin configuration:

        <plugin>
            <groupId>org.apache.avro</groupId>
            <artifactId>avro-maven-plugin</artifactId>
            <version>1.10.0</version>
            <configuration>
                <stringType>String</stringType>
                <customConversions>
                    org.apache.avro.Conversions$UUIDConversion,org.apache.avro.data.TimeConversions$LocalTimestampMillisConversion
                </customConversions>
            </configuration>
            <executions>
                <execution>
                    <phase>generate-sources</phase>
                    <goals>
                        <goal>schema</goal>
                    </goals>
                    <configuration>
                        <sourceDirectory>${project.basedir}/src/main/avro/</sourceDirectory>
                        <outputDirectory>${project.build.directory}/generated-sources/avro/</outputDirectory>
                    </configuration>
                </execution>
            </executions>
        </plugin>
