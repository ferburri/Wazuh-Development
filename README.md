# Wazuh-Development

The main objective of this repository consists of implementing custom decoders and rules in order to improve the capacities of Wazuh in order to detect more alerts.

## Wazuh Log Ingestion and Rule Infraestructure

### Part 0: How log ingestion works in Wazuh
The proccess that a log line follow to be an alert is:

Firstly, the Wazuh Agent Daemon sends a log line for example of MySQL to the  Wazuh Manager, and here, the log line follows three phasses:

We are going to use as example this MySQL log line: `2023-04-28T07:33:21.108843Z        24 Query     SELECT * FROM users where username='' or 4=4 -- ' and password='1234'`

1. Pre-Decoding: Wazuh automatically looks for fields in the log, in our example, it recognizes the timestamp and the query.
2. Decoding: Wazuh looks matches in all the decoders, if some decoder match, it will return the fields that are configured in the decoer.
3. Rule Matching: At this time, the rule can be invoked by the decoder if it's configured in the rule, else, it can be matched, and return some fields, like the id, level, description, groups, mitre...

### Part 1: SQL injection decoder and rule
The purpose of this part, is to detect SQL injections on the MySQL/MariaDB queries, rigth now, the detector is basic, but we are going to improve it in the future.

A example log that is supposed to match: "2023-04-28T07:33:21.108843Z        24 Query     SELECT * FROM users where username='' or 4=4 -- ' and password='1234'"

A example log that is not supposed to match: "2023-04-28T07:33:21.108843Z        24 Query     SELECT * FROM users where username='Paco' and password='Paco'"

At this time, the decoder matchs log lines that have more than four simple quotes, that method is weak, and will be improved.
