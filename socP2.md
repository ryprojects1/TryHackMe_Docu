# Splunk has three main components: Forwarder, Indexer, and Search Head. These components work together to help us search and analyze the data. 
Splunk Forwarder
Splunk Forwarder is a lightweight agent installed on the endpoint intended to be monitored, and its main task is to collect the data and send it to the Splunk instance. It does not affect the endpoint's performance as it takes a few resources to process.
. Some of the key data sources are:

Web server generating web traffic.
Windows machine generating Windows Event Logs, PowerShell, and Sysmon data.
Linux host generating host-centric logs.
Database generating DB connection requests, responses, and errors.
Splunk Indexer plays the main role in processing the data it receives from forwarders. It parses and normalizes the data into field-value pairs, categorizes it, and stores the results as events, making the processed data easy to search and analyze.

Search Head
Splunk Search Head is the place within the Search & Reporting App where users can search the indexed logs, as shown below. The searches are done using the SPL (Search Processing Language), a powerful query language for searching indexed data. When the user performs a search, the request is sent to the indexer, and the relevant events are returned as field-value pairs.

 the Splunk Bar, we have the following options available:

Messages: View system-level notifications and messages.
Settings: Configure Splunk instance settings.
Activity: Review the progress of search jobs and processes.
Help: View tutorials and documentation.
Find: Search across the App.
