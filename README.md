# CPPM2Wazuh
Aruba's ClearPass Policy Manager logs can be easily exported to ELK via the great work done by @njohnsn with his [ClearPassAndELK](https://github.com/njohnsn/ClearPassAndELK) - thanks Neil :+1:

Once you have got the events lines, you still need to parse them to let Elasticsearch doing all its magic; one of the common solutions is to augment ELK prepending a Wazuh (formerly OSSEC) stage: roughly speaking, this way you can write **Decoders** to parse the events and **Rules** to take following actions (usually related to alerts triggering and log conservation policies). Dealing with processes, Rules can be very organization-specific so to discourage their sharing due to low reusability and disclosures worries; however Decoders are definitely more geeky :wink: being nothing more that huge regexes, so I think [the ones I have written for CPPM](https://github.com/baro77/CPPM2Wazuh/blob/main/clearpass.xml) could be useful to someone else.

Neil's exporter provides many filters for 20+ kind of logs, for now my decoders deal with ```CPPM_to_ELK_RADIUS_Session```, ```CPPM_to_ELK_RADIUS_Accounting```, ```CPPM_to_ELK_RADIUS_Accounting_Detail``` (namely RADIUS Authentications, RADACCT, and per-"non standard attribute" accounting messages)... more to follow, keep checking this repo.

I have written them for Wazuh version 3.10; and, given the diversity in providing infos via RADIUS protocol by NASes, let me specify that the decoders have been *tested* on flows coming from HP Procurve, HPE Aruba and Comware7 switches, Aruba Mobility Controllers, Fortinet devices and Pulse VPNs, Eduroam's Italian FLR proxies.
In this context *tested* means that at the time of writing decoders match all events they have been receiving (no unparsed ones): to check if that's the lucky case for you as well, I suggest to use Kibana to match any event with ```predecoder.hostname``` set to your CPPM IP but missing ```data.CPPM_Syslog_ExportFilter``` field (meaning no suitable decoder has been found). 

### NOTE
Due to privacy concerns, the example-events used to write each regular expression have been removed from the file: human inspection of decoders results definitely awkward, sorry.
