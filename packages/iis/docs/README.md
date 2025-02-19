# IIS (Internet Information Services) integration

The IIS (Internet Information Services) integration allows you to monitor your IIS Web servers.
IIS is a secure, reliable, and scalable Web server that provides an easy to manage platform for developing and hosting Web applications and services.

Use the IIS integration to collect data. Then visualize that data in Kibana, create alerts to notify you if something goes wrong, and reference metrics and logs when troubleshooting an issue.

For example, you could:

* Use IIS System/Process counters like the overall server and CPU usage for the IIS Worker Process and memory to understand how much memory is currently being used and how much is available.
* Use IIS performance counters like _Web Service: Bytes Received/Sec_ and _Web Service: Bytes Sent/Sec_ to track to identify potential spikes in traffic.
* Use IIS Web Service Cache counters to monitor user mode cache and output cache.

## Data streams

The IIS integration collects two types of data streams: logs and metrics.

**Logs** help you keep a record of events happening on your IIS Web servers.
Log data streams collected by the IIS integration include `access` and `error`.
Find more details in [Logs](#logs-reference).

**Metrics** give you insight into the state of your IIS Web servers.
Metric data streams collected by the IIS integration include `webserver`, `website`, and `application_pool`.
Find more details in [Metrics](#metrics-reference).

## Requirements

You need Elasticsearch for storing and searching your data and Kibana for visualizing and managing it.
You can use our hosted Elasticsearch Service on Elastic Cloud, which is recommended, or self-manage the Elastic Stack on your hardware.

## Setup

<!-- Any prerequisite instructions -->

For step-by-step instructions on how to set up an integration, see the
[Getting started](https://www.elastic.co/guide/en/welcome-to-elastic/current/getting-started-observability.html) guide.

<!-- Additional set up instructions -->
For more information on configuring IIS logging, refer to the [Microsoft documentation](https://learn.microsoft.com/en-us/iis/manage/provisioning-and-managing-iis/configure-logging-in-iis).

## Logs

### Compatibility

The IIS module has been tested with logs from version 7.5, 8 and version 10.

### access

This data stream will collect and parse access IIS logs. The supported log format is W3C. The W3C log format is customizable with different fields.

The IIS ships logs with few fields by default and if the user is interested in customizing the selection, the IIS Manager provides ability to add new fields for logging.

IIS integration offers certain field combinations shipped automatically into Elasticsearch using ingest pipelines. The supported formats are listed below,

#### Default Logging:

    - Fields: date time s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Referer) sc-status sc-substatus sc-win32-status time-taken

#### Custom Logging:

    - Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status time-taken

    - Fields: date time s-sitename s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs-version cs(User-Agent) cs(cookie) cs(Referer) sc-status sc-substatus sc-win32-status sc-bytes, cs-bytes time-taken

    - Fields: date time s-sitename s-computername s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs-version cs(User-Agent) cs(cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes, cs-bytes time-taken

    - Fields: date time s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status time-taken

    - Fields: date time s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status sc-bytes, cs-bytes time-taken

    - Fields: date time s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(cookie) cs(Referer) sc-status sc-substatus sc-win32-status sc-bytes, cs-bytes time-taken

    - Fields: date time s-computername s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status sc-bytes, cs-bytes time-taken

An example event for `access` looks as following:

```json
{
    "agent": {
        "name": "DESKTOP-RFOOE09",
        "id": "db17f9fb-5bcb-4116-a009-79a1bb7d4820",
        "type": "filebeat",
        "ephemeral_id": "3f65b650-b6a3-4694-83b3-0c324a60809d",
        "version": "8.0.0"
    },
    "temp": {},
    "destination": {
        "address": "127.0.0.1",
        "port": 80,
        "ip": "127.0.0.1"
    },
    "source": {
        "address": "127.0.0.1",
        "ip": "127.0.0.1"
    },
    "url": {
        "path": "/"
    },
    "iis": {
        "access": {
            "sub_status": 3,
            "win32_status": 5
        }
    },
    "@timestamp": "2018-11-19T15:24:54.000Z",
    "ecs": {
        "version": "8.5.1"
    },
    "related": {
        "ip": [
            "127.0.0.1",
            "127.0.0.1"
        ]
    },
    "http": {
        "request": {
            "method": "GET"
        },
        "response": {
            "status_code": 401
        }
    },
    "event": {
        "duration": 725000000,
        "created": "2020-07-08T11:40:14.112Z",
        "kind": "event",
        "category": [
            "web",
            "network"
        ],
        "type": [
            "connection"
        ],
        "outcome": "failure"
    },
    "user_agent": {
        "original": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36",
        "os": {
            "name": "Windows",
            "version": "10",
            "full": "Windows 10"
        },
        "name": "Chrome",
        "device": {
            "name": "Other"
        },
        "version": "70.0.3538.102"
    }
}
```

The fields reported are:

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| destination.address | Some event destination addresses are defined ambiguously. The event will sometimes list an IP, a domain or a unix socket.  You should always store the raw address in the `.address` field. Then it should be duplicated to `.ip` or `.domain`, depending on which one it is. | keyword |
| destination.domain | The domain name of the destination system. This value may be a host name, a fully qualified domain name, or another host naming format. The value may derive from the original event or be added from enrichment. | keyword |
| destination.ip | IP address of the destination (IPv4 or IPv6). | ip |
| destination.port | Port of the destination. | long |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |
| error.message | Error message. | match_only_text |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset | constant_keyword |
| event.duration | Duration of the event in nanoseconds. If event.start and event.end are known this value should be the difference between the end and start time. | long |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data coming in at a regular interval or not. | keyword |
| event.module | Event module | constant_keyword |
| event.original | Raw text message of entire event. Used to demonstrate log integrity or where the full log message (before splitting it up in multiple parts) may be required, e.g. for reindex. This field is not indexed and doc_values are disabled. It cannot be searched, but it can be retrieved from `_source`. If users wish to override this and index this field, please see `Field data types` in the `Elasticsearch Reference`. | keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host mac addresses. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| http.request.body.bytes | Size in bytes of the request body. | long |
| http.request.method | HTTP request method. The value should retain its casing from the original event. For example, `GET`, `get`, and `GeT` are all considered valid values for this field. | keyword |
| http.request.referrer | Referrer for this HTTP request. | keyword |
| http.response.body.bytes | Size in bytes of the response body. | long |
| http.response.status_code | HTTP response status code. | long |
| http.version | HTTP version. | keyword |
| iis.access.cookie | The content of the cookie sent or received, if any. | keyword |
| iis.access.server_name | The name of the server on which the log file entry was generated. | keyword |
| iis.access.site_name | The site name and instance number. | keyword |
| iis.access.sub_status | The HTTP substatus code. | long |
| iis.access.win32_status | The Windows status code. | long |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| network.forwarded_ip | Host IP address when the source IP address is the proxy. | ip |
| related.hosts | All hostnames or other host identifiers seen on your event. Example identifiers include FQDNs, domain names, workstation names, or aliases. | keyword |
| related.ip | All of the IPs seen on your event. | ip |
| related.user | All the user names or other user identifiers seen on the event. | keyword |
| source.address | Some event source addresses are defined ambiguously. The event will sometimes list an IP, a domain or a unix socket.  You should always store the raw address in the `.address` field. Then it should be duplicated to `.ip` or `.domain`, depending on which one it is. | keyword |
| source.as.number | Unique number allocated to the autonomous system. The autonomous system number (ASN) uniquely identifies each network on the Internet. | long |
| source.as.organization.name | Organization name. | keyword |
| source.as.organization.name.text | Multi-field of `source.as.organization.name`. | match_only_text |
| source.geo.city_name | City name. | keyword |
| source.geo.continent_name | Name of the continent. | keyword |
| source.geo.country_iso_code | Country ISO code. | keyword |
| source.geo.country_name | Country name. | keyword |
| source.geo.location | Longitude and latitude. | geo_point |
| source.geo.region_iso_code | Region ISO code. | keyword |
| source.geo.region_name | Region name. | keyword |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |
| tags | List of keywords used to tag each event. | keyword |
| url.domain | Domain of the url, such as "www.elastic.co". In some cases a URL may refer to an IP and/or port directly, without a domain name. In this case, the IP address would go to the `domain` field. If the URL contains a literal IPv6 address enclosed by `[` and `]` (IETF RFC 2732), the `[` and `]` characters should also be captured in the `domain` field. | keyword |
| url.extension | The field contains the file extension from the original request url, excluding the leading dot. The file extension is only set if it exists, as not every url has a file extension. The leading period must not be included. For example, the value must be "png", not ".png". Note that when the file name has multiple extensions (example.tar.gz), only the last one should be captured ("gz", not "tar.gz"). | keyword |
| url.original | Unmodified original url as seen in the event source. Note that in network monitoring, the observed URL may be a full URL, whereas in access logs, the URL is often just represented as a path. This field is meant to represent the URL as it was observed, complete or not. | wildcard |
| url.original.text | Multi-field of `url.original`. | match_only_text |
| url.path | Path of the request, such as "/search". | wildcard |
| url.query | The query field describes the query string of the request, such as "q=elasticsearch". The `?` is excluded from the query string. If a URL contains no `?`, there is no query field. If there is a `?` but no query, the query field exists with an empty string. The `exists` query can be used to differentiate between the two cases. | keyword |
| user.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| user.name | Short name or login of the user. | keyword |
| user.name.text | Multi-field of `user.name`. | match_only_text |
| user_agent.device.name | Name of the device. | keyword |
| user_agent.name | Name of the user agent. | keyword |
| user_agent.original | Unparsed user_agent string. | keyword |
| user_agent.original.text | Multi-field of `user_agent.original`. | match_only_text |
| user_agent.os.full | Operating system name, including the version or code name. | keyword |
| user_agent.os.full.text | Multi-field of `user_agent.os.full`. | match_only_text |
| user_agent.os.name | Operating system name, without the version. | keyword |
| user_agent.os.name.text | Multi-field of `user_agent.os.name`. | match_only_text |
| user_agent.os.version | Operating system version as a raw string. | keyword |
| user_agent.version | Version of the user agent. | keyword |


### error

This data stream will collect and parse error IIS logs.

An example event for `error` looks as following:

```json
{
    "agent": {
        "name": "DESKTOP-RFOOE09",
        "id": "db17f9fb-5bcb-4116-a009-79a1bb7d4820",
        "type": "filebeat",
        "ephemeral_id": "3f65b650-b6a3-4694-83b3-0c324a60809d",
        "version": "8.0.0"
    },
    "destination": {
        "address": "::1%0",
        "port": 80,
        "ip": "::1"
    },
    "source": {
        "address": "::1%0",
        "port": 59827,
        "ip": "::1"
    },
    "iis": {
        "error": {
            "reason_phrase": "Timer_ConnectionIdle"
        }
    },
    "@timestamp": "2020-06-30T13:56:46.000Z",
    "ecs": {
        "version": "8.5.1"
    },
    "related": {
        "ip": [
            "::1",
            "::1"
        ]
    },
    "event": {
        "created": "2020-07-08T11:40:13.768Z",
        "kind": "event",
        "category": [
            "web",
            "network"
        ],
        "type": [
            "connection"
        ]
    }
}
```

The fields reported are:

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| destination.address | Some event destination addresses are defined ambiguously. The event will sometimes list an IP, a domain or a unix socket.  You should always store the raw address in the `.address` field. Then it should be duplicated to `.ip` or `.domain`, depending on which one it is. | keyword |
| destination.domain | The domain name of the destination system. This value may be a host name, a fully qualified domain name, or another host naming format. The value may derive from the original event or be added from enrichment. | keyword |
| destination.ip | IP address of the destination (IPv4 or IPv6). | ip |
| destination.port | Port of the destination. | long |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |
| error.message | Error message. | match_only_text |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset | constant_keyword |
| event.duration | Duration of the event in nanoseconds. If event.start and event.end are known this value should be the difference between the end and start time. | long |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data coming in at a regular interval or not. | keyword |
| event.module | Event module | constant_keyword |
| event.original | Raw text message of entire event. Used to demonstrate log integrity or where the full log message (before splitting it up in multiple parts) may be required, e.g. for reindex. This field is not indexed and doc_values are disabled. It cannot be searched, but it can be retrieved from `_source`. If users wish to override this and index this field, please see `Field data types` in the `Elasticsearch Reference`. | keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host mac addresses. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| http.request.method | HTTP request method. The value should retain its casing from the original event. For example, `GET`, `get`, and `GeT` are all considered valid values for this field. | keyword |
| http.response.status_code | HTTP response status code. | long |
| http.version | HTTP version. | keyword |
| iis.error.queue_name | The IIS application pool name. | keyword |
| iis.error.reason_phrase | The HTTP reason phrase. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| related.ip | All of the IPs seen on your event. | ip |
| related.user | All the user names or other user identifiers seen on the event. | keyword |
| source.address | Some event source addresses are defined ambiguously. The event will sometimes list an IP, a domain or a unix socket.  You should always store the raw address in the `.address` field. Then it should be duplicated to `.ip` or `.domain`, depending on which one it is. | keyword |
| source.as.number | Unique number allocated to the autonomous system. The autonomous system number (ASN) uniquely identifies each network on the Internet. | long |
| source.as.organization.name | Organization name. | keyword |
| source.as.organization.name.text | Multi-field of `source.as.organization.name`. | match_only_text |
| source.geo.city_name | City name. | keyword |
| source.geo.continent_name | Name of the continent. | keyword |
| source.geo.country_iso_code | Country ISO code. | keyword |
| source.geo.country_name | Country name. | keyword |
| source.geo.location | Longitude and latitude. | geo_point |
| source.geo.region_iso_code | Region ISO code. | keyword |
| source.geo.region_name | Region name. | keyword |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |
| source.port | Port of the source. | long |
| tags | List of keywords used to tag each event. | keyword |
| url.domain | Domain of the url, such as "www.elastic.co". In some cases a URL may refer to an IP and/or port directly, without a domain name. In this case, the IP address would go to the `domain` field. If the URL contains a literal IPv6 address enclosed by `[` and `]` (IETF RFC 2732), the `[` and `]` characters should also be captured in the `domain` field. | keyword |
| url.extension | The field contains the file extension from the original request url, excluding the leading dot. The file extension is only set if it exists, as not every url has a file extension. The leading period must not be included. For example, the value must be "png", not ".png". Note that when the file name has multiple extensions (example.tar.gz), only the last one should be captured ("gz", not "tar.gz"). | keyword |
| url.original | Unmodified original url as seen in the event source. Note that in network monitoring, the observed URL may be a full URL, whereas in access logs, the URL is often just represented as a path. This field is meant to represent the URL as it was observed, complete or not. | wildcard |
| url.original.text | Multi-field of `url.original`. | match_only_text |
| url.path | Path of the request, such as "/search". | wildcard |
| url.query | The query field describes the query string of the request, such as "q=elasticsearch". The `?` is excluded from the query string. If a URL contains no `?`, there is no query field. If there is a `?` but no query, the query field exists with an empty string. The `exists` query can be used to differentiate between the two cases. | keyword |



## Metrics

### webserver

The `webserver` data stream allows users to retrieve aggregated metrics for the entire web server.

An example event for `webserver` looks as following:

```json
{
    "@timestamp": "2020-07-08T11:42:12.102Z",
    "service": {
        "type": "iis"
    },
    "ecs": {
        "version": "8.5.1"
    },
    "agent": {
        "name": "DESKTOP-RFOOE09",
        "type": "metricbeat",
        "version": "8.0.0",
        "ephemeral_id": "8ade3582-e6ab-4664-ba27-52b3d46953e3",
        "id": "3b73ebb6-c6ea-4354-b1f3-240ac1aa072c"
    },
    "iis": {
        "webserver": {
            "asp_net": {
                "application_restarts": 0,
                "request_wait_time": 0
            },
            "asp_net_application": {
                "requests_in_application_queue": 0,
                "pipeline_instance_count": 2,
                "requests_executing": 0
            },
            "network": {
                "total_get_requests": 52,
                "total_anonymous_users": 52,
                "current_connections": 2,
                "anonymous_users_per_sec": 0,
                "service_uptime": 1721919.0,
                "total_post_requests": 0,
                "total_non_anonymous_users": 0,
                "bytes_received_per_sec": 0,
                "total_delete_requests": 0,
                "current_non_anonymous_users": 0,
                "bytes_sent_per_sec": 0,
                "total_bytes_received": 33151,
                "current_anonymous_users": 0,
                "post_requests_per_sec": 0,
                "total_connection_attempts": 23,
                "delete_requests_per_sec": 0,
                "get_requests_per_sec": 0,
                "maximum_connections": 6,
                "total_bytes_sent": 903338
            },
            "process": {
                "io_write_operations_per_sec": 5.7271735422265,
                "worker_process_count": 2,
                "private_bytes": 1.06692608E8,
                "page_faults_per_sec": 1.0738450391674688,
                "virtual_bytes": 2.222663852032E12,
                "io_read_operations_per_sec": 5.7271735422265
            },
            "cache": {
                "current_files_cached": 2,
                "file_cache_misses": 70,
                "total_files_cached": 15,
                "output_cache_current_memory_usage": 0,
                "file_cache_hits": 18,
                "uri_cache_hits": 14,
                "output_cache_total_hits": 0,
                "output_cache_current_items": 0,
                "current_file_cache_memory_usage": 696,
                "current_uris_cached": 1,
                "uri_cache_misses": 62,
                "maximum_file_cache_memory_usage": 99453,
                "output_cache_total_misses": 76,
                "total_uris_cached": 10
            }
        }
    },
    "event": {
        "dataset": "iis.webserver",
        "module": "iis",
        "duration": 1205854900
    },
    "metricset": {
        "period": 10000,
        "name": "webserver"
    }
}
```

The fields reported are:

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| agent.id |  | keyword |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |  |  |
| event.dataset | Event dataset | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| iis.webserver.asp_net.application_restarts | Number of applications restarts. | float |  | gauge |
| iis.webserver.asp_net.request_wait_time | Request wait time. | long |  |  |
| iis.webserver.asp_net_application.errors_total_per_sec | Total number of errors per sec. | float |  | gauge |
| iis.webserver.asp_net_application.pipeline_instance_count | The pipeline instance count. | float |  | gauge |
| iis.webserver.asp_net_application.requests_executing | Number of requests executing. | float |  | gauge |
| iis.webserver.asp_net_application.requests_in_application_queue | Number of requests in the application queue. | float |  |  |
| iis.webserver.asp_net_application.requests_per_sec | Number of requests per sec. | float |  | gauge |
| iis.webserver.cache.current_file_cache_memory_usage | The current file cache memory usage size. | float |  |  |
| iis.webserver.cache.current_files_cached | The number of current files cached. | float |  |  |
| iis.webserver.cache.current_uris_cached | The number of current uris cached. | float |  |  |
| iis.webserver.cache.file_cache_hits | The number of file cache hits. | float |  |  |
| iis.webserver.cache.file_cache_misses | The number of file cache misses. | float |  |  |
| iis.webserver.cache.maximum_file_cache_memory_usage | The max file cache size. | float |  |  |
| iis.webserver.cache.output_cache_current_items | The number of output cache current items. | float |  |  |
| iis.webserver.cache.output_cache_current_memory_usage | The output cache memory usage size. | float |  |  |
| iis.webserver.cache.output_cache_total_hits | The output cache total hits count. | float |  |  |
| iis.webserver.cache.output_cache_total_misses | The output cache total misses count. | float |  |  |
| iis.webserver.cache.total_files_cached | the total number of files cached. | float |  |  |
| iis.webserver.cache.total_uris_cached | The total number of URIs cached. | float |  |  |
| iis.webserver.cache.uri_cache_hits | The number of URIs cached hits. | float |  |  |
| iis.webserver.cache.uri_cache_misses | The number of URIs cache misses. | float |  |  |
| iis.webserver.network.anonymous_users_per_sec | The number of anonymous users per sec. | float |  | gauge |
| iis.webserver.network.bytes_received_per_sec | The size of bytes received per sec. | float | byte | gauge |
| iis.webserver.network.bytes_sent_per_sec | The size of bytes sent per sec. | float | byte | gauge |
| iis.webserver.network.current_anonymous_users | The number of current anonymous users. | float |  |  |
| iis.webserver.network.current_connections | The number of current connections. | float |  |  |
| iis.webserver.network.current_non_anonymous_users | The number of current non anonymous users. | float |  |  |
| iis.webserver.network.delete_requests_per_sec | Number of DELETE requests per sec. | float |  | gauge |
| iis.webserver.network.get_requests_per_sec | Number of GET requests per sec. | float |  | gauge |
| iis.webserver.network.maximum_connections | Number of maximum connections. | float |  | counter |
| iis.webserver.network.post_requests_per_sec | Number of POST requests per sec. | float |  | gauge |
| iis.webserver.network.service_uptime | Service uptime. | float |  |  |
| iis.webserver.network.total_anonymous_users | Total number of anonymous users. | float |  | counter |
| iis.webserver.network.total_bytes_received | Total size of bytes received. | float | byte | counter |
| iis.webserver.network.total_bytes_sent | Total size of bytes sent. | float | byte | counter |
| iis.webserver.network.total_connection_attempts | The total number of connection attempts. | float |  |  |
| iis.webserver.network.total_delete_requests | The total number of DELETE requests. | float |  | counter |
| iis.webserver.network.total_get_requests | The total number of GET requests. | float |  | counter |
| iis.webserver.network.total_non_anonymous_users | The total number of non anonymous users. | float |  | counter |
| iis.webserver.network.total_post_requests | The total number of POST requests. | float |  | counter |
| iis.webserver.process.cpu_usage_perc | The CPU usage percentage. | float |  | gauge |
| iis.webserver.process.handle_count | The number of handles. | float |  |  |
| iis.webserver.process.io_read_operations_per_sec | IO read operations per sec. | float |  | gauge |
| iis.webserver.process.io_write_operations_per_sec | IO write operations per sec. | float |  | gauge |
| iis.webserver.process.page_faults_per_sec | Memory page faults. | float |  | gauge |
| iis.webserver.process.private_bytes | Memory private bytes. | float | byte | gauge |
| iis.webserver.process.thread_count | The number of threads. | long |  |  |
| iis.webserver.process.virtual_bytes | Memory virtual bytes. | float | byte | gauge |
| iis.webserver.process.worker_process_count | Number of worker processes running. | float |  |  |
| iis.webserver.process.working_set | Memory working set. | float |  |  |
| service.address | Address where data about this service was collected from. This should be a URI, network address (ipv4:port or [ipv6]:port) or a resource path (sockets). | keyword |  |  |
| service.type | The type of the service data is collected from. The type can be used to group and correlate logs and metrics from one service type. Example: If logs or metrics are collected from Elasticsearch, `service.type` would be `elasticsearch`. | keyword |  |  |


### website

This data stream will collect metrics of specific sites, users can configure which websites they want to monitor, else, all are considered.

An example event for `website` looks as following:

```json
{
    "@timestamp": "2020-07-08T11:40:22.114Z",
    "ecs": {
        "version": "8.5.1"
    },
    "iis": {
        "website": {
            "name": "test2.local",
            "network": {
                "total_put_requests": 0,
                "total_get_requests": 11,
                "service_uptime": 1721807.0,
                "total_bytes_sent": 135739,
                "maximum_connections": 4,
                "total_connection_attempts": 7,
                "total_post_requests": 0,
                "total_bytes_received": 4250,
                "current_connections": 0,
                "total_delete_requests": 0
            }
        }
    },
    "event": {
        "dataset": "iis.website",
        "module": "iis",
        "duration": 5008200
    },
    "metricset": {
        "name": "website",
        "period": 10000
    },
    "service": {
        "type": "iis"
    },
    "agent": {
        "type": "metricbeat",
        "version": "8.0.0",
        "ephemeral_id": "8ade3582-e6ab-4664-ba27-52b3d46953e3",
        "id": "3b73ebb6-c6ea-4354-b1f3-240ac1aa072c",
        "name": "DESKTOP-RFOOE09"
    }
}
```

The fields reported are:

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| agent.id |  | keyword |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |  |  |
| event.dataset | Event dataset | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| iis.website.name | website name | keyword |  |  |
| iis.website.network.bytes_received_per_sec | The bytes received per sec size. | float | byte | gauge |
| iis.website.network.bytes_sent_per_sec | The bytes sent per sec size. | float | byte | gauge |
| iis.website.network.current_connections | The number of current connections. | float |  |  |
| iis.website.network.delete_requests_per_sec | The number of DELETE requests per sec. | float |  | gauge |
| iis.website.network.get_requests_per_sec | The number of GET requests per sec. | float |  | gauge |
| iis.website.network.maximum_connections | The number of maximum connections. | float |  |  |
| iis.website.network.post_requests_per_sec | The number of POST requests per sec. | float |  | gauge |
| iis.website.network.put_requests_per_sec | The number of PUT requests per sec. | float |  | gauge |
| iis.website.network.service_uptime | The service uptime. | float |  |  |
| iis.website.network.total_bytes_received | The total number of bytes received. | float | byte | counter |
| iis.website.network.total_bytes_sent | The  total number of bytes sent. | float | byte | counter |
| iis.website.network.total_connection_attempts | The total number of connection attempts. | float |  | counter |
| iis.website.network.total_delete_requests | The total number of DELETE requests. | float |  | counter |
| iis.website.network.total_get_requests | The total number of GET requests. | float |  | counter |
| iis.website.network.total_post_requests | The total number of POST requests. | float |  | counter |
| iis.website.network.total_put_requests | The total number of PUT requests. | float |  | counter |
| service.address | Address where data about this service was collected from. This should be a URI, network address (ipv4:port or [ipv6]:port) or a resource path (sockets). | keyword |  |  |
| service.type | The type of the service data is collected from. The type can be used to group and correlate logs and metrics from one service type. Example: If logs or metrics are collected from Elasticsearch, `service.type` would be `elasticsearch`. | keyword |  |  |


### application_pool

This data stream will collect metrics of specific application pools, users can configure which websites they want to monitor, else, all are considered.

An example event for `application_pool` looks as following:

```json
{
    "@timestamp": "2020-07-08T11:41:31.048Z",
    "event": {
        "dataset": "iis.application_pool",
        "module": "iis",
        "duration": 397142600
    },
    "agent": {
        "name": "DESKTOP-RFOOE09",
        "type": "metricbeat",
        "version": "8.0.0",
        "ephemeral_id": "8ade3582-e6ab-4664-ba27-52b3d46953e3",
        "id": "3b73ebb6-c6ea-4354-b1f3-240ac1aa072c"
    },
    "service": {
        "type": "iis"
    },
    "iis": {
        "application_pool": {
            "name": "DefaultAppPool",
            "net_clr": {
                "total_exceptions_thrown": 0
            },
            "process": {
                "thread_count": 30,
                "handle_count": 466,
                "private_bytes": 7.151616E7
            }
        }
    },
    "ecs": {
        "version": "8.5.1"
    },
    "metricset": {
        "period": 10000,
        "name": "application_pool"
    }
}
```

The fields reported are:

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| agent.id |  | keyword |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |  |  |
| event.dataset | Event dataset | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| iis.application_pool.name | application pool name | keyword |  |  |
| iis.application_pool.net_clr.filters_per_sec | Number of filters per sec. | float |  | gauge |
| iis.application_pool.net_clr.finallys_per_sec | The number of finallys per sec. | float |  | gauge |
| iis.application_pool.net_clr.throw_to_catch_depth_per_sec | Throw to catch depth count per sec. | float |  | gauge |
| iis.application_pool.net_clr.total_exceptions_thrown | Total number of exceptions thrown. | long |  | counter |
| iis.application_pool.process.cpu_usage_perc | The CPU usage percentage. | float | s | gauge |
| iis.application_pool.process.handle_count | The number of handles. | long |  |  |
| iis.application_pool.process.io_read_operations_per_sec | IO read operations per sec. | float |  | gauge |
| iis.application_pool.process.io_write_operations_per_sec | IO write operations per sec. | float |  | gauge |
| iis.application_pool.process.page_faults_per_sec | Memory page faults. | float |  | gauge |
| iis.application_pool.process.private_bytes | Memory private bytes. | float | byte | gauge |
| iis.application_pool.process.thread_count | The number of threads. | long |  | counter |
| iis.application_pool.process.virtual_bytes | Memory virtual bytes. | float | byte | gauge |
| iis.application_pool.process.working_set | Memory working set. | float |  |  |
| service.address | Address where data about this service was collected from. This should be a URI, network address (ipv4:port or [ipv6]:port) or a resource path (sockets). | keyword |  |  |
| service.type | The type of the service data is collected from. The type can be used to group and correlate logs and metrics from one service type. Example: If logs or metrics are collected from Elasticsearch, `service.type` would be `elasticsearch`. | keyword |  |  |
