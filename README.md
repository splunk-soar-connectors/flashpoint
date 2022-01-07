[comment]: # "Auto-generated SOAR connector documentation"
# Flashpoint

Publisher: Flashpoint  
Connector Version: 1\.0\.2  
Product Vendor: Flashpoint  
Product Name: Flashpoint  
Product Version Supported (regex): "\.\*"  
Minimum Product Version: 4\.8\.24304  

This app implements the investigative actions for the Flashpoint on the Phantom Platform

[comment]: # " File: redme.html"
[comment]: # ""
[comment]: # "    Copyright (c) Flashpoint, 2020"
[comment]: # ""
[comment]: # "    This unpublished material is proprietary to Flashpoint."
[comment]: # "    All rights reserved. The methods and"
[comment]: # "    techniques described herein are considered trade secrets"
[comment]: # "    and/or confidential. Reproduction or distribution, in whole"
[comment]: # "    or in part, is forbidden except by express written permission"
[comment]: # "    of Flashpoint."
[comment]: # ""
[comment]: # "    Licensed under Apache 2.0 (https://www.apache.org/licenses/LICENSE-2.0.txt)"
[comment]: # ""
[comment]: # ""
## Explanation of Asset Configuration Parameters

The asset configuration parameters affect \[test connectivity\] and all the other actions of the
application. Below are the explanation and usage of all those parameters.

-   **Base URL -** The URL to connect to the Flashpoint server.
-   **API Token -** The API token of the user.
-   **Retry Wait Period (in seconds) -** The value of this parameter defines the waiting period in
    seconds for which to hold the current execution of the action on receiving the “500 Internal
    Server Error” and then, retry the same API call after the waiting period is exhausted. This
    ensures that the integration provides a mechanism of attempting to overcome the intermittent
    “500 Internal Server Error”. It allows only non-zero positive integer values as input. The
    default value is 5 seconds.
-   **Number Of Retries -** The value of this parameter defines the number of attempts for which the
    action will keep on retrying if the Flashpoint API continuously returns the “500 Internal Server
    Error”. If the intermittent error gets eliminated before the number of retries gets exhausted,
    then, the action execution will continue along its workflow with the next set of API calls and
    if the intermittent error is still persistent and all the number of retries are exhausted, then,
    the action will fail with the latest error message being displayed. It allows only zero or
    positive integer values as input. The default value is 1 retry.
-   **Session Timeout -** This is an optional asset configuration parameter. The value of this
    parameter will be used as the session timeout value in the ‘Get Compromised Credentials’ and
    ‘Run Query’ actions while using the session scrolling pagination. The default value is 2 minutes
    and the maximum allowed value is 60 minutes.

## Steps to generate API Token

1.  Go to [Flashpoint](https://fp.tools/) .
2.  Select **APIs & Integrations** from the left side panel.
3.  Under the **FLASHPOINT API** section, select **Manage API Tokens** .
4.  Click on the **GENERATE TOKEN** .
5.  Enter a **Token Label** and your current FPTools credentials in the **Username** and
    **Password** fields in the appeared **Generate API Token** prompt.
6.  Click on the **GENERATE** button.
7.  This will generate a new API token and will display it in the **GENERATE API TOKEN** section on
    the page.

  **Note-** Save your generated API token somewhere secure, as you will no longer be able to
retrieve this key after leaving this page.

## Explanation of Flashpoint Actions' Parameters

1.  ### Test Connectivity (Action Workflow Details)

    -   This action will test the connectivity of the Phantom server to the Flashpoint instance by
        making an initial API call to the Indicators API using the provided asset configuration
        parameters.
    -   The action validates the provided asset configuration parameters. Based on the API call
        response, the appropriate success and failure message will be displayed when the action gets
        executed.

      

2.  ### List Indicators

    -   **<u>Action Parameter</u> ​ - Attribute Types**

        -   This parameter enables search by attribute types. It is an optional action parameter. It
            supports the comma-separated list of attribute types values. Each value from the
            provided comma-separated list must correspond to one of the MISP types, a list of which
            can be found [here](https://www.circl.lu/doc/misp/categories-and-types/#types) .
        -   **Examples:**
            -   Get recent md5, sha1, or source IP indicators
                -   Attribute Types = md5,sha1,ip-src

          
          

    -   **<u>Action Parameter</u> ​ - Query**

        -   This parameter will be used for free text searching. It is an optional parameter. You
            can also provide different queries to filter out indicators results.
        -   **Examples:**
            -   Filtering results based on the field value
                -   Query = category:”Payload Delivery”
            -   Free text search(when using multiple words, use a + instead of space, and for
                specific word search use “test text” (inverted double quotes) in the query action
                parameter.)
                -   Query = gandcrab+ransomware
                -   Query = “test text”

          
          

    -   **<u>Action Parameter</u> - Limit**

        -   This parameter is used to limit the number of indicator results. The default value
            is 500. If the limit is not provided, it will fetch by default 500 indicator results.

          
          

    -   **<u>Notes</u> -** The user will have to provide URL value in the "Attribute Types" action
        parameter and the URL value enclosed in double-quotes in the "Query" parameter if they want
        to search for an IoC having a specific URL value. This does not work correctly if the user
        provides the URL value without double-quotes in the "Query" parameter. This is based on the
        current API behavior of the Flashpoint.

      

3.  ### Search Indicators

    -   **<u>Action Parameter</u> ​ - Attribute Type and Attribute Value**

        -   These parameters are required parameters. They will be used to retrieve specific
            indicator results based on the provided values.
        -   **Examples:**
            -   Get indicator matching a specific hash value

                -   Attribute Type = md5 (any of md5,sha1,sha256, etc.)
                -   Attribute Value= 16139ce9025274a388a4281fef65049e

                  
                  

            -   Get indicator matching a specific filename

                -   Attribute Type = filename
                -   Attribute Value= "PLEASE-CHECK”

                <u>Note</u> - In the above example, without the double quotes around the filename,
                it will search for every filename that matches 'PLEASE'. The hyphen/space will be
                considered as the end of the search value and it will search for indicators matching
                the value until the first encountered hyphen/space.

                  
                  

            -   Get indicator matching a specific source IP Address

                -   Attribute Type = ip-src
                -   Attribute Value = 111.255.198.92

                  
                  

            -   Get indicator matching a specific URL value
                -   Attribute Type = url
                -   Attribute Value=
                    http://ww1.gadmobs.com/?subid1=bf5b0786-272c-11e9-b8c7-e15edf920d61

                <u>Note</u> - Internally, this URL value passed within the inverted comma(for ad-hoc
                fixation) in the request parameters. Because without the inverted comma, the server
                responded with the Internal Server Error unnecessarily.

          
          

    -   **<u>Action Parameter</u> ​ - Limit**

        -   This parameter is used to limit the number of indicator results. The default value
            is 500. If the limit is not provided, it will fetch by default 500 indicator results.

          
          

    -   **<u>Notes</u> -** This action is not working with the valid value of IoC type which
        consists of pipe symbol(\|) in its name. In case of searching the IoC of that type, you can
        use \[run query\] or \[list indicators\] actions by providing an appropriate query in the
        "Query" action parameter. Below are the examples:

        <u>For \[run query\] action</u> :  
          
        Search for IoC value which consists of pipe symbol(\|) in the IoC attribute type

        -   <u>Usage</u> :
        -   Query = +basetypes:indicator_attribute +type:"\<ioc_type>" +value.\\\*:\<ioc_value>

          

        -   <u>Example</u> :
        -   Query = +basetypes:indicator_attribute +type:"ip-dst\|port" +value.\\\*:5.79.68.110\|80

        <u>For \[list indicators\] action</u> :  
          
        Search for IoC value which consists of pipe symbol(\|) in the IoC attribute type

        -   <u>Usage</u> :
        -   Attribute Types = \<ioc_type>
        -   Query = +value.\\\*:\<ioc_value>

          

        -   <u>Example</u> :
        -   Attribute Types = ip-dst\|port
        -   Query = +value.\\\*:"5.79.68.110\|80"

      

4.  ### List Reports

    -   **<u>Action Parameter</u> ​ - Limit**

        -   This is an optional parameter. It is used to limit the number of fetched intelligence
            reports. The default value is 500. If the limit is not provided, it will fetch by
            default 500 intelligence reports.

          
          
        **<u>Note</u> -** Based on the current API analysis, the endpoint for this action fetches a
        huge set of data. Hence, the action run might take more time for a larger limit value.

      

5.  ### Get Report

    -   **<u>Action Parameter</u> ​ - Report ID**
        -   This is a required parameter. It is a Flashpoint intelligence report ID.
        -   **Examples:**
            -   Fetch an intelligence report having the provided report ID value
                -   Report ID = wrh9BCZETzu3AO3CUopOlw

      

6.  ### List Related Reports

    -   **<u>Action Parameter</u> ​ - Report ID**

        -   This is a required parameter. It is a Flashpoint intelligence report ID.
        -   **Examples:**
            -   Fetch default 500 related intelligence reports for the provided report ID
                -   Report ID = wrh9BCZETzu3AO3CUopOlw
                -   Limit = Keep it empty

          
          

    -   **<u>Action Parameter</u> ​ - Limit**

        -   This is an optional parameter. It is used to limit the number of fetched intelligence
            reports. The default value is 500. If the limit is not provided, it will fetch by
            default 500 intelligence reports.

          
          
        **<u>Note</u> -** Based on the current API analysis, the endpoint for this action fetches a
        huge set of data. Hence, the action run might take more time for a larger limit value.

      

7.  ### Get Compromised Credentials

    -   **<u>Action Parameter</u> ​ - Filter**

        -   This parameter will be used for filtering the data of credentials sightings on the
            Flashpoint instance. It is an optional parameter. If not given, it will get all the
            compromised credentials. A few sample values of the filter action parameter are listed
            below.
            -   +is_fresh:true (search for only new credential sightings)
            -   +breach.first_observed_at.date-time:\[now-30d TO now\] (search for credential
                sightings which are discovered in the last month based on the date provided from the
                source of this credential sightings data)
            -   +breach.fpid:nIbeDs_VXyKedBmuhFEaGQ (search for all credential sightings in a
                Breach)
            -   +email:username (search for a username)
            -   +email.keyword:username@domain.com (search for an email address)
            -   +domain.keyword:domain.com (search for credentials sightings data of a particular
                domain)
        -   **Examples:**
            -   Search for credential sightings of the given domain and that are discovered in the
                last month based on the date provided from the source of this credential sightings
                data
                -   Filter = +domain.keyword:domain.com+breach.first_observed_at.date-time:\[now-30d
                    TO now\]
            -   Search for credential sightings of the given domain and that are discovered in the
                last month based on the date of indexing of the data into the Flashpoint server
                -   Filter = +domain.keyword:domain.com+header\_.indexed_at:\[now-30d TO now\]
            -   Search for credential sightings which are discovered in the last month based on the
                date of indexing of the data into the Flashpoint server
                -   Filter = +header\_.indexed_at:\[now-30d TO now\]
            -   Search for credential sightings which are discovered in between the provided
                timestamps based on the date provided from the source of this credential sightings
                data
                -   Filter = +breach.first_observed_at.timestamp:\[1234567890 TO 1234567890\]
        -   **Usage:**
            -   For making filter parameter value
                -   Query= +basetypes:credential-sighting\<filter>

                Here, the filter is any supported values by the search API endpoint.

          
          

    -   **<u>Action Parameter</u> ​ - Limit**
        -   This parameter is used to limit the number of fetched compromised credentials. The
            default value is 500. If the limit is not provided, it will fetch by default 500
            compromised credentials. The internal pagination logic for fetching a large number of
            compromised credentials implements the scrolling session-based Credentials All Search
            APIs.

      

8.  ### Run Query

    -   **<u>Action Parameter</u> ​ - Query**

        -   This parameter will be used to search across all fields in the marketplace data by
            appending terms to it or limit searches to individual fields by appending \<field
            name>:\<value> to the ‘Query’ parameter. The queries supported by action are listed
            below.
            -   Credential breach queries (+basetypes:breach)
            -   CVE queries (+basetypes:cve)
            -   Card queries (+basetypes:card)
            -   Paste queries (+basetypes:paste)
            -   Chat queries (+basetypes:generic-product)
            -   Indicator attribute queries (+basetypes:indicator_attribute)
            -   Credential sightings queries (+basetypes:credential-sighting)
            -   Vulnerability queries (+basetypes:vulnerability)
            -   Conversation queries (+basetypes:conversation)
            -   Chan queries (+basetypes:chan)
            -   Blog queries (+basetypes:blog)
            -   Reddit queries (+basetypes:reddit)
            -   Forum queries (+basetypes:forum)
        -   **Examples:**
            -   Search for "Analyst Research" breaches
                -   Query= +basetypes:breach+source_type:"Analyst Research"
            -   Search for "testing" across all free-form fields (message body, channel profile,
                channel name, and user name) for chat queries
                -   Query = +basetypes:chat+testing
            -   Search for credential sightings of the given domain and that are discovered in the
                last month based on the date provided from the source of this credential sightings
                data
                -   Filter =
                    +basetypes:credential-sighting+domain.keyword:domain.com+breach.first_observed_at.date-time:\[now-30d
                    TO now\]
            -   Search for all search results which are discovered in the last month based on the
                date of indexing of the data into the Flashpoint server
                -   Filter = +header\_.indexed_at:\[now-30d TO now\]
            -   Filter all search results by ISO date/time range based on the date provided from the
                source of this search data
                -   Query = +created_at.date-time:\["2018-10-24T10:05:10+00:00" TO
                    "2018-10-26T10:05:10+00:00"\]
            -   Filter results by Unix time for all paste results based on the date provided from
                the source of this paste search data
                -   Query = +basetypes:paste+created_at.timestamp:\[1234567890 TO 1234567890\]
        -   **Usage:**
            -   For making query parameter value
                -   Query= \<basetypes_query>\<search_filter>

                Here, basetypes_query and search_filter are any supported values by the search API
                endpoint.

          
          

    -   **<u>Action Parameter</u> ​ - Limit**
        -   This parameter is used to limit the number of fetched all search data. The default value
            is 500. If the limit is not provided, it will fetch by default 500 search items. The
            internal pagination logic for fetching a large number of search items implements the
            scrolling session-based All Search APIs.

      


### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a Flashpoint asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**base\_url** |  required  | string | Base URL
**api\_token** |  required  | password | API Token
**wait\_timeout\_period** |  optional  | numeric | Retry Wait Period\(in seconds\)
**no\_of\_retries** |  optional  | numeric | Number Of Retries
**session\_timeout** |  optional  | numeric | Session Timeout\(in minutes\)

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate the asset configuration for connectivity using supplied configuration  
[list reports](#action-list-reports) - Fetch a list of all the intelligence reports from the Flashpoint Platform  
[get report](#action-get-report) - Fetch a specific intelligence report from the Flashpoint Platform for the provided report ID  
[list related reports](#action-list-related-reports) - Fetch a list of all the related intelligence reports from the Flashpoint Platform for the provided report ID  
[get compromised credentials](#action-get-compromised-credentials) - Fetch a list of all the Credential Sightings from the Flashpoint Platform  
[run query](#action-run-query) - Fetch the data by performing a universal search from the Flashpoint Platform  
[list indicators](#action-list-indicators) - Fetch a list of IoCs that occur in the context of an event from the Flashpoint Platform  
[search indicators](#action-search-indicators) - Fetch an IoC value of a specific attribute type from the list of available IoCs on the Flashpoint Platform  

## action: 'test connectivity'
Validate the asset configuration for connectivity using supplied configuration

Type: **test**  
Read only: **True**

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'list reports'
Fetch a list of all the intelligence reports from the Flashpoint Platform

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**limit** |  optional  | Maximum number of reports to be fetched \(default\: 500\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.limit | numeric | 
action\_result\.data\.\*\.asset\_ids | string | 
action\_result\.data\.\*\.assets | string | 
action\_result\.data\.\*\.body | string | 
action\_result\.data\.\*\.id | string |  `fp report id` 
action\_result\.data\.\*\.ingested\_at | string | 
action\_result\.data\.\*\.is\_featured | boolean | 
action\_result\.data\.\*\.notified\_at | string | 
action\_result\.data\.\*\.platform\_url | string |  `url` 
action\_result\.data\.\*\.posted\_at | string | 
action\_result\.data\.\*\.processed\_body | string | 
action\_result\.data\.\*\.processed\_summary | string | 
action\_result\.data\.\*\.published\_status | string | 
action\_result\.data\.\*\.sources\.\*\.original | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.platform\_url | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.source | string | 
action\_result\.data\.\*\.sources\.\*\.source\_id | string | 
action\_result\.data\.\*\.sources\.\*\.title | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.type | string | 
action\_result\.data\.\*\.summary | string | 
action\_result\.data\.\*\.tags | string | 
action\_result\.data\.\*\.title | string | 
action\_result\.data\.\*\.title\_asset | string | 
action\_result\.data\.\*\.title\_asset\_id | string | 
action\_result\.data\.\*\.updated\_at | string | 
action\_result\.data\.\*\.version\_posted\_at | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary\.total\_reports | numeric | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'get report'
Fetch a specific intelligence report from the Flashpoint Platform for the provided report ID

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**report\_id** |  required  | Flashpoint intelligence report ID | string |  `fp report id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.report\_id | string |  `fp report id` 
action\_result\.data\.\*\.asset\_ids | string | 
action\_result\.data\.\*\.assets | string | 
action\_result\.data\.\*\.body | string | 
action\_result\.data\.\*\.id | string |  `fp report id` 
action\_result\.data\.\*\.ingested\_at | string | 
action\_result\.data\.\*\.is\_featured | boolean | 
action\_result\.data\.\*\.notified\_at | string | 
action\_result\.data\.\*\.platform\_url | string |  `url` 
action\_result\.data\.\*\.posted\_at | string | 
action\_result\.data\.\*\.processed\_body | string | 
action\_result\.data\.\*\.processed\_summary | string | 
action\_result\.data\.\*\.published\_status | string | 
action\_result\.data\.\*\.sources\.\*\.original | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.platform\_url | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.source | string | 
action\_result\.data\.\*\.sources\.\*\.source\_id | string | 
action\_result\.data\.\*\.sources\.\*\.title | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.type | string | 
action\_result\.data\.\*\.summary | string | 
action\_result\.data\.\*\.tags | string | 
action\_result\.data\.\*\.title | string | 
action\_result\.data\.\*\.title\_asset | string | 
action\_result\.data\.\*\.title\_asset\_id | string | 
action\_result\.data\.\*\.updated\_at | string | 
action\_result\.data\.\*\.version\_posted\_at | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'list related reports'
Fetch a list of all the related intelligence reports from the Flashpoint Platform for the provided report ID

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**report\_id** |  required  | Flashpoint intelligence report ID | string |  `fp report id` 
**limit** |  optional  | Maximum number of reports to be fetched \(default\: 500\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.limit | numeric | 
action\_result\.parameter\.report\_id | string |  `fp report id` 
action\_result\.data\.\*\.asset\_ids | string | 
action\_result\.data\.\*\.assets | string | 
action\_result\.data\.\*\.body | string | 
action\_result\.data\.\*\.id | string |  `fp report id` 
action\_result\.data\.\*\.ingested\_at | string | 
action\_result\.data\.\*\.is\_featured | boolean | 
action\_result\.data\.\*\.notified\_at | string | 
action\_result\.data\.\*\.platform\_url | string |  `url` 
action\_result\.data\.\*\.posted\_at | string | 
action\_result\.data\.\*\.processed\_body | string | 
action\_result\.data\.\*\.processed\_summary | string | 
action\_result\.data\.\*\.published\_status | string | 
action\_result\.data\.\*\.sources\.\*\.original | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.platform\_url | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.source | string | 
action\_result\.data\.\*\.sources\.\*\.source\_id | string | 
action\_result\.data\.\*\.sources\.\*\.title | string |  `url` 
action\_result\.data\.\*\.sources\.\*\.type | string | 
action\_result\.data\.\*\.summary | string | 
action\_result\.data\.\*\.tags | string | 
action\_result\.data\.\*\.title | string | 
action\_result\.data\.\*\.title\_asset | string | 
action\_result\.data\.\*\.title\_asset\_id | string | 
action\_result\.data\.\*\.updated\_at | string | 
action\_result\.data\.\*\.version\_posted\_at | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary\.total\_related\_reports | numeric | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'get compromised credentials'
Fetch a list of all the Credential Sightings from the Flashpoint Platform

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**filter** |  optional  | Filtering the data of credentials sightings | string | 
**limit** |  optional  | Maximum number of reports to be fetched \(default\: 500\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.filter | string | 
action\_result\.parameter\.limit | numeric | 
action\_result\.data\.\*\.\_id | string | 
action\_result\.data\.\*\.\_source\.basetypes | string |  `fp query basetypes` 
action\_result\.data\.\*\.\_source\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.breach\.basetypes | string |  `fp query basetypes` 
action\_result\.data\.\*\.\_source\.breach\.breach\_type | string | 
action\_result\.data\.\*\.\_source\.breach\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.breach\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.breach\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.breach\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.breach\.fpid | string | 
action\_result\.data\.\*\.\_source\.breach\.source | string | 
action\_result\.data\.\*\.\_source\.breach\.source\_type | string | 
action\_result\.data\.\*\.\_source\.breach\.title | string | 
action\_result\.data\.\*\.\_source\.breach\.victim | string | 
action\_result\.data\.\*\.\_source\.credential\_record\_fpid | string | 
action\_result\.data\.\*\.\_source\.customer\_id | string | 
action\_result\.data\.\*\.\_source\.domain | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.email | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.\_source\.extraction\_id | string | 
action\_result\.data\.\*\.\_source\.extraction\_record\_id | string | 
action\_result\.data\.\*\.\_source\.fpid | string | 
action\_result\.data\.\*\.\_source\.header\_\.indexed\_at | numeric | 
action\_result\.data\.\*\.\_source\.is\_fresh | boolean | 
action\_result\.data\.\*\.\_source\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.password | string | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_lowercase | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_number | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_symbol | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_uppercase | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.length | numeric | 
action\_result\.data\.\*\.\_source\.password\_complexity\.probable\_hash\_algorithms | string | 
action\_result\.data\.\*\.\_source\.times\_seen | numeric | 
action\_result\.data\.\*\.\_type | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary\.total\_results | numeric | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'run query'
Fetch the data by performing a universal search from the Flashpoint Platform

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**query** |  required  | Search across all fields in the marketplace or free text search | string |  `fp query basetypes` 
**limit** |  optional  | Maximum number of reports to be fetched \(default\: 500\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.limit | numeric | 
action\_result\.parameter\.query | string |  `fp query basetypes` 
action\_result\.data\.\*\.\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.authors | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.description | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.galaxy\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.external\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.kill\_chain | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.mitre\_data\_sources | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.mitre\_platforms | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.refs | string |  `url` 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.source | string |  `url` 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.tag\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.tag\_name | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.type | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.value | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.version | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.description | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.icon | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.type | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.Galaxy\.\*\.version | string | 
action\_result\.data\.\*\.\_source\.Event\.Org\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.Org\.name | string | 
action\_result\.data\.\*\.\_source\.Event\.Org\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.Orgc\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.Orgc\.name | string | 
action\_result\.data\.\*\.\_source\.Event\.Orgc\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.Org\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.Org\.name | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.Org\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.Orgc\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.Orgc\.name | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.Orgc\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.analysis | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.date | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.distribution | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.info | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.org\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.orgc\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.published | boolean | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.threat\_level\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.timestamp | string | 
action\_result\.data\.\*\.\_source\.Event\.RelatedEvent\.\*\.Event\.uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.Tag\.\*\.colour | string | 
action\_result\.data\.\*\.\_source\.Event\.Tag\.\*\.exportable | boolean | 
action\_result\.data\.\*\.\_source\.Event\.Tag\.\*\.hide\_tag | boolean | 
action\_result\.data\.\*\.\_source\.Event\.Tag\.\*\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.Tag\.\*\.name | string |  `file name` 
action\_result\.data\.\*\.\_source\.Event\.Tag\.\*\.user\_id | boolean | 
action\_result\.data\.\*\.\_source\.Event\.analysis | string | 
action\_result\.data\.\*\.\_source\.Event\.attribute\_count | string | 
action\_result\.data\.\*\.\_source\.Event\.date | string | 
action\_result\.data\.\*\.\_source\.Event\.disable\_correlation | boolean | 
action\_result\.data\.\*\.\_source\.Event\.distribution | string | 
action\_result\.data\.\*\.\_source\.Event\.event\_creator\_email | string |  `email` 
action\_result\.data\.\*\.\_source\.Event\.extends\_uuid | string | 
action\_result\.data\.\*\.\_source\.Event\.fpid | string | 
action\_result\.data\.\*\.\_source\.Event\.id | string | 
action\_result\.data\.\*\.\_source\.Event\.info | string | 
action\_result\.data\.\*\.\_source\.Event\.locked | boolean | 
action\_result\.data\.\*\.\_source\.Event\.org\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.orgc\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.proposal\_email\_lock | boolean | 
action\_result\.data\.\*\.\_source\.Event\.publish\_timestamp | string | 
action\_result\.data\.\*\.\_source\.Event\.published | boolean | 
action\_result\.data\.\*\.\_source\.Event\.sharing\_group\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.threat\_level\_id | string | 
action\_result\.data\.\*\.\_source\.Event\.timestamp | string | 
action\_result\.data\.\*\.\_source\.Event\.uuid | string | 
action\_result\.data\.\*\.\_source\.Tag\.\*\.colour | string | 
action\_result\.data\.\*\.\_source\.Tag\.\*\.exportable | boolean | 
action\_result\.data\.\*\.\_source\.Tag\.\*\.hide\_tag | boolean | 
action\_result\.data\.\*\.\_source\.Tag\.\*\.id | string | 
action\_result\.data\.\*\.\_source\.Tag\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.Tag\.\*\.user\_id | boolean | 
action\_result\.data\.\*\.\_source\.account\_domain | string | 
action\_result\.data\.\*\.\_source\.account\_holder\_information\.full\_name | string | 
action\_result\.data\.\*\.\_source\.account\_holder\_information\.location\.address | string | 
action\_result\.data\.\*\.\_source\.account\_holder\_information\.location\.country\.raw | string | 
action\_result\.data\.\*\.\_source\.account\_organization | string | 
action\_result\.data\.\*\.\_source\.account\_type | string | 
action\_result\.data\.\*\.\_source\.balance | numeric | 
action\_result\.data\.\*\.\_source\.bank\_name | string | 
action\_result\.data\.\*\.\_source\.base\.basetypes | string |  `fp query basetypes` 
action\_result\.data\.\*\.\_source\.base\.fpid | string | 
action\_result\.data\.\*\.\_source\.base\.native\_id | string | 
action\_result\.data\.\*\.\_source\.base\.raw | string | 
action\_result\.data\.\*\.\_source\.base\.release\_date\.date\-time | string | 
action\_result\.data\.\*\.\_source\.base\.release\_date\.raw | string | 
action\_result\.data\.\*\.\_source\.base\.release\_date\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.base\.title | string | 
action\_result\.data\.\*\.\_source\.basetypes | string |  `fp query basetypes` 
action\_result\.data\.\*\.\_source\.bin | numeric | 
action\_result\.data\.\*\.\_source\.board\.name | string | 
action\_result\.data\.\*\.\_source\.board\.native\_id | string | 
action\_result\.data\.\*\.\_source\.board\.site\.behavior | string | 
action\_result\.data\.\*\.\_source\.board\.site\.href | string | 
action\_result\.data\.\*\.\_source\.board\.site\.target | string | 
action\_result\.data\.\*\.\_source\.board\.title | string | 
action\_result\.data\.\*\.\_source\.board\.type | string | 
action\_result\.data\.\*\.\_source\.body\.enrichments\.cves | string | 
action\_result\.data\.\*\.\_source\.body\.enrichments\.domains | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.body\.enrichments\.hashtags | string | 
action\_result\.data\.\*\.\_source\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.body\.enrichments\.links\.\*\.href | string |  `url` 
action\_result\.data\.\*\.\_source\.body\.enrichments\.social\_media\_handles | string | 
action\_result\.data\.\*\.\_source\.body\.raw | string |  `url` 
action\_result\.data\.\*\.\_source\.body\.text/html\+sanitized | string |  `url` 
action\_result\.data\.\*\.\_source\.body\.text/html\-sanitized | string | 
action\_result\.data\.\*\.\_source\.body\.text/plain | string |  `url` 
action\_result\.data\.\*\.\_source\.breach\.basetypes | string | 
action\_result\.data\.\*\.\_source\.breach\.breach\_type | string | 
action\_result\.data\.\*\.\_source\.breach\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.breach\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.breach\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.breach\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.breach\.fpid | string | 
action\_result\.data\.\*\.\_source\.breach\.source | string | 
action\_result\.data\.\*\.\_source\.breach\.source\_type | string | 
action\_result\.data\.\*\.\_source\.breach\.title | string | 
action\_result\.data\.\*\.\_source\.breach\.victim | string | 
action\_result\.data\.\*\.\_source\.breach\_intersections\.\*\.count | numeric | 
action\_result\.data\.\*\.\_source\.breach\_intersections\.\*\.dump | string | 
action\_result\.data\.\*\.\_source\.breach\_intersections\.\*\.title | string | 
action\_result\.data\.\*\.\_source\.breach\_intersections\.count | numeric | 
action\_result\.data\.\*\.\_source\.breach\_intersections\.dump | string | 
action\_result\.data\.\*\.\_source\.breach\_intersections\.title | string | 
action\_result\.data\.\*\.\_source\.card\_number | string | 
action\_result\.data\.\*\.\_source\.card\_type | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.date\_of\_birth\.raw | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.email | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.first | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.full\_name | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.is\_date\_of\_birth\_available | boolean | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.is\_email\_available | boolean | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.is\_mothers\_maiden\_name\_available | boolean | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.is\_phone\_number\_available | boolean | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.is\_social\_security\_number\_available | boolean | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.last | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.address | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.city | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.country\.abbreviation | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.country\.full\_name | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.country\.raw | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.raw | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.region\.abbreviation | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.region\.full\_name | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.region\.raw | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.location\.zip\_code | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.phone\_number | string | 
action\_result\.data\.\*\.\_source\.cardholder\_information\.social\_security\_number\.full | string | 
action\_result\.data\.\*\.\_source\.category | string | 
action\_result\.data\.\*\.\_source\.container\.admins\_count | numeric | 
action\_result\.data\.\*\.\_source\.container\.basetypes | string | 
action\_result\.data\.\*\.\_source\.container\.body\.enrichments\.domains | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.container\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.container\.body\.enrichments\.links\.\*\.href | string |  `url`  `ip` 
action\_result\.data\.\*\.\_source\.container\.body\.raw | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.body\.text/html\+sanitized | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.body\.text/plain | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.category | string | 
action\_result\.data\.\*\.\_source\.container\.container\.basetypes | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.bins | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.bitcoin\_addresses | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.domains | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.email\_addresses | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.facebook\_urls | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.hashtags | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.links\.\*\.href | string |  `url`  `ip` 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.pans | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.partial\_cards | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.enrichments\.social\_media\_handles | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.text/html\+sanitized | string | 
action\_result\.data\.\*\.\_source\.container\.container\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.container\.container\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.container\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.container\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.container\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.container\.container\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.container\.first\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.container\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.container\.fpid | string | 
action\_result\.data\.\*\.\_source\.container\.container\.icon\_url | string | 
action\_result\.data\.\*\.\_source\.container\.container\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.container\.container\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.container\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.container\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.container\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.container\.container\.name | string | 
action\_result\.data\.\*\.\_source\.container\.container\.native\_id | string | 
action\_result\.data\.\*\.\_source\.container\.container\.num\_subscribers | numeric | 
action\_result\.data\.\*\.\_source\.container\.container\.region | string | 
action\_result\.data\.\*\.\_source\.container\.container\.server\_owner\.id | string | 
action\_result\.data\.\*\.\_source\.container\.container\.server\_owner\.username | string | 
action\_result\.data\.\*\.\_source\.container\.container\.source\_uri | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.container\.title | string | 
action\_result\.data\.\*\.\_source\.container\.container\.type | string | 
action\_result\.data\.\*\.\_source\.container\.container\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.container\.container\.verification\_level | string | 
action\_result\.data\.\*\.\_source\.container\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.description | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.enrichments\.domains | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.container\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.container\.enrichments\.links\.\*\.href | string |  `url`  `ip` 
action\_result\.data\.\*\.\_source\.container\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.first\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.fpid | string | 
action\_result\.data\.\*\.\_source\.container\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.container\.kicked\_count | numeric | 
action\_result\.data\.\*\.\_source\.container\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.container\.name | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.native\_id | string | 
action\_result\.data\.\*\.\_source\.container\.num\_replies | numeric | 
action\_result\.data\.\*\.\_source\.container\.participants\_count | numeric | 
action\_result\.data\.\*\.\_source\.container\.permission\_overrides\.\*\.overrides | string | 
action\_result\.data\.\*\.\_source\.container\.permission\_overrides\.\*\.role\.id | string | 
action\_result\.data\.\*\.\_source\.container\.permission\_overrides\.\*\.role\.name | string | 
action\_result\.data\.\*\.\_source\.container\.raw\_href | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.reputation\.number\_of\_downvotes | numeric | 
action\_result\.data\.\*\.\_source\.container\.reputation\.number\_of\_upvotes | numeric | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.avatar\_uri\.href | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.basetypes | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.flair\.flair\_text | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.is\_admin | boolean | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.names\.aliases | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.base\_uris | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.basetypes | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.title | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.source\_uri | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.site\_actor\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.container\.source\_uri | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.title | string |  `url` 
action\_result\.data\.\*\.\_source\.container\.topic | string | 
action\_result\.data\.\*\.\_source\.container\.type | string | 
action\_result\.data\.\*\.\_source\.container\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.container\.username | string |  `url`  `user name` 
action\_result\.data\.\*\.\_source\.container\_position\.index\_number | numeric | 
action\_result\.data\.\*\.\_source\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.credential\_record\_fpid | string | 
action\_result\.data\.\*\.\_source\.credit\_cards\.\*\.raw | string | 
action\_result\.data\.\*\.\_source\.customer\_id | string | 
action\_result\.data\.\*\.\_source\.cve\.basetypes | string | 
action\_result\.data\.\*\.\_source\.cve\.fpid | string | 
action\_result\.data\.\*\.\_source\.cve\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.basetypes | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.body\.enrichments\.cves | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.body\.enrichments\.links\.\*\.href | string |  `url` 
action\_result\.data\.\*\.\_source\.cve\.mitre\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.body\.text/html\-sanitized | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.fpid | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.native\_id | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.phase | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.base\_uris | string |  `url` 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.basetypes | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.title | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.status | string | 
action\_result\.data\.\*\.\_source\.cve\.mitre\.title | string | 
action\_result\.data\.\*\.\_source\.cve\.native\_id | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.assigner | string |  `email` 
action\_result\.data\.\*\.\_source\.cve\.nist\.basetypes | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.body\.enrichments\.cves | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.body\.enrichments\.links\.\*\.href | string |  `url` 
action\_result\.data\.\*\.\_source\.cve\.nist\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.body\.text/html\-sanitized | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.configurations\.\*\.cpe23\_uri | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.configurations\.\*\.version\_end\_including | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.access\_complexity | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.access\_vector | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.authentication | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.availability\_impact | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.base\_score | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.confidentiality\_impact | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.exploitability\_score | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.impact\_score | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.integrity\_impact | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.severity | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv2\.vector\_string | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.attack\_complexity | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.attack\_vector | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.availability\_impact | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.base\_score | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.confidentiality\_impact | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.exploitability\_score | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.impact\_score | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.integrity\_impact | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.privileges\_required | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.scope | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.severity | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.user\_interaction | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.cvssv3\.vector\_string | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.fpid | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.native\_id | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.products\.\*\.product\_name | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.products\.\*\.vendor\_name | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.references\.\*\.name | string |  `url` 
action\_result\.data\.\*\.\_source\.cve\.nist\.references\.\*\.refsource | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.references\.\*\.tags | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.references\.\*\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.base\_uris | string |  `url` 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.basetypes | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.title | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.title | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.updated\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.cve\.nist\.updated\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.cve\.nist\.vulnerability\_types | string | 
action\_result\.data\.\*\.\_source\.cve\.title | string | 
action\_result\.data\.\*\.\_source\.cvv | numeric | 
action\_result\.data\.\*\.\_source\.deleted | boolean | 
action\_result\.data\.\*\.\_source\.disable\_correlation | boolean | 
action\_result\.data\.\*\.\_source\.distribution | string | 
action\_result\.data\.\*\.\_source\.domain | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.email | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.\_source\.email\_domain | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.enrichments\.domains | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.enrichments\.hashtags | string | 
action\_result\.data\.\*\.\_source\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.enrichments\.links\.\*\.href | string |  `url` 
action\_result\.data\.\*\.\_source\.enrichments\.social\_media\_handles | string | 
action\_result\.data\.\*\.\_source\.expiration | string | 
action\_result\.data\.\*\.\_source\.expires\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.expires\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.expires\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.extraction\_id | string | 
action\_result\.data\.\*\.\_source\.extraction\_record\_id | string | 
action\_result\.data\.\*\.\_source\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.first\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.fpid | string | 
action\_result\.data\.\*\.\_source\.has\_credit\_card | boolean | 
action\_result\.data\.\*\.\_source\.has\_email\_access | boolean | 
action\_result\.data\.\*\.\_source\.header\_\.collected\_fpid | string | 
action\_result\.data\.\*\.\_source\.header\_\.indexed\_at | numeric | 
action\_result\.data\.\*\.\_source\.header\_\.ingested\_at | numeric | 
action\_result\.data\.\*\.\_source\.header\_\.is\_visible | boolean | 
action\_result\.data\.\*\.\_source\.header\_\.observed\_at | numeric | 
action\_result\.data\.\*\.\_source\.header\_\.source | string |  `url` 
action\_result\.data\.\*\.\_source\.header\_\.source\_fpid | string | 
action\_result\.data\.\*\.\_source\.header\_\.source\_keyword | string | 
action\_result\.data\.\*\.\_source\.header\_\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.id | string | 
action\_result\.data\.\*\.\_source\.is\_cvv\_available | boolean | 
action\_result\.data\.\*\.\_source\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.is\_edited | boolean | 
action\_result\.data\.\*\.\_source\.is\_fresh | boolean | 
action\_result\.data\.\*\.\_source\.is\_media | boolean | 
action\_result\.data\.\*\.\_source\.is\_pin\_available | boolean | 
action\_result\.data\.\*\.\_source\.is\_track1\_available | boolean | 
action\_result\.data\.\*\.\_source\.is\_verified | boolean | 
action\_result\.data\.\*\.\_source\.is\_verified\_by\_visa | boolean | 
action\_result\.data\.\*\.\_source\.last4 | string | 
action\_result\.data\.\*\.\_source\.last\_checked\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.last\_checked\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.last\_checked\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.level | string | 
action\_result\.data\.\*\.\_source\.location\.country\.abbreviation | string | 
action\_result\.data\.\*\.\_source\.location\.country\.full\_name | string | 
action\_result\.data\.\*\.\_source\.media\.author | string |  `url` 
action\_result\.data\.\*\.\_source\.media\.basetypes | string | 
action\_result\.data\.\*\.\_source\.media\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.media\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.media\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.media\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.media\.description | string | 
action\_result\.data\.\*\.\_source\.media\.filename | string | 
action\_result\.data\.\*\.\_source\.media\.fpid | string | 
action\_result\.data\.\*\.\_source\.media\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.media\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.media\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.media\.mime\_type | string | 
action\_result\.data\.\*\.\_source\.media\.native\_id | string | 
action\_result\.data\.\*\.\_source\.media\.phash | string | 
action\_result\.data\.\*\.\_source\.media\.sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.\_source\.media\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.media\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.media\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.media\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.media\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.media\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.media\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.media\.site\.title | string | 
action\_result\.data\.\*\.\_source\.media\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.media\.size | numeric | 
action\_result\.data\.\*\.\_source\.media\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.media\.storage\_uri | string | 
action\_result\.data\.\*\.\_source\.media\.title | string | 
action\_result\.data\.\*\.\_source\.media\.type | string | 
action\_result\.data\.\*\.\_source\.message\_count\.count | numeric | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.container\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.message\_count\.first\_resource\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.first\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.native\_id | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.container\.title | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.native\_id | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.container\.title | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.title | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.message\_count\.last\_resource\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.mitre\.basetypes | string | 
action\_result\.data\.\*\.\_source\.mitre\.body\.enrichments\.cves | string | 
action\_result\.data\.\*\.\_source\.mitre\.body\.enrichments\.links\.\*\.href | string |  `url`  `ip` 
action\_result\.data\.\*\.\_source\.mitre\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.mitre\.body\.text/html\-sanitized | string | 
action\_result\.data\.\*\.\_source\.mitre\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.mitre\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.mitre\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.mitre\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.mitre\.fpid | string | 
action\_result\.data\.\*\.\_source\.mitre\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.mitre\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.mitre\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.mitre\.native\_id | string | 
action\_result\.data\.\*\.\_source\.mitre\.phase | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.base\_uris | string |  `url` 
action\_result\.data\.\*\.\_source\.mitre\.site\.basetypes | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.title | string | 
action\_result\.data\.\*\.\_source\.mitre\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.mitre\.status | string | 
action\_result\.data\.\*\.\_source\.mitre\.title | string | 
action\_result\.data\.\*\.\_source\.native\_id | string |  `md5` 
action\_result\.data\.\*\.\_source\.new\_records | numeric | 
action\_result\.data\.\*\.\_source\.nist\.assigner | string |  `email` 
action\_result\.data\.\*\.\_source\.nist\.basetypes | string | 
action\_result\.data\.\*\.\_source\.nist\.body\.enrichments\.cves | string | 
action\_result\.data\.\*\.\_source\.nist\.body\.enrichments\.links\.\*\.href | string |  `url`  `ip` 
action\_result\.data\.\*\.\_source\.nist\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.nist\.body\.text/html\-sanitized | string | 
action\_result\.data\.\*\.\_source\.nist\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.nist\.configurations\.\*\.cpe23\_uri | string | 
action\_result\.data\.\*\.\_source\.nist\.configurations\.\*\.version\_end\_including | string |  `ip` 
action\_result\.data\.\*\.\_source\.nist\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.nist\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.nist\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.access\_complexity | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.access\_vector | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.authentication | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.availability\_impact | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.base\_score | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.confidentiality\_impact | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.exploitability\_score | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.impact\_score | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.integrity\_impact | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.severity | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv2\.vector\_string | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.attack\_complexity | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.attack\_vector | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.availability\_impact | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.base\_score | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.confidentiality\_impact | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.exploitability\_score | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.impact\_score | numeric | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.integrity\_impact | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.privileges\_required | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.scope | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.severity | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.user\_interaction | string | 
action\_result\.data\.\*\.\_source\.nist\.cvssv3\.vector\_string | string | 
action\_result\.data\.\*\.\_source\.nist\.fpid | string | 
action\_result\.data\.\*\.\_source\.nist\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.nist\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.nist\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.nist\.native\_id | string | 
action\_result\.data\.\*\.\_source\.nist\.products\.\*\.product\_name | string | 
action\_result\.data\.\*\.\_source\.nist\.products\.\*\.vendor\_name | string | 
action\_result\.data\.\*\.\_source\.nist\.references\.\*\.name | string |  `url` 
action\_result\.data\.\*\.\_source\.nist\.references\.\*\.refsource | string | 
action\_result\.data\.\*\.\_source\.nist\.references\.\*\.tags | string | 
action\_result\.data\.\*\.\_source\.nist\.references\.\*\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.nist\.site\.base\_uris | string |  `url` 
action\_result\.data\.\*\.\_source\.nist\.site\.basetypes | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.title | string | 
action\_result\.data\.\*\.\_source\.nist\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.nist\.title | string | 
action\_result\.data\.\*\.\_source\.nist\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.nist\.updated\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.nist\.updated\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.nist\.vulnerability\_types | string | 
action\_result\.data\.\*\.\_source\.num\_replies | numeric | 
action\_result\.data\.\*\.\_source\.object\_id | string | 
action\_result\.data\.\*\.\_source\.object\_relation | string | 
action\_result\.data\.\*\.\_source\.old\_records | numeric | 
action\_result\.data\.\*\.\_source\.parent\_comment\.native\_id | string | 
action\_result\.data\.\*\.\_source\.parent\_comment\.site\.behavior | string | 
action\_result\.data\.\*\.\_source\.parent\_comment\.site\.href | string | 
action\_result\.data\.\*\.\_source\.parent\_comment\.site\.target | string | 
action\_result\.data\.\*\.\_source\.parent\_comment\.type | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.basetypes | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.fpid | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.native\_id | string |  `url` 
action\_result\.data\.\*\.\_source\.parent\_message\.num\_replies | numeric | 
action\_result\.data\.\*\.\_source\.parent\_message\.site\_actor\.avatar\_uri\.href | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.parent\_message\.site\_actor\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.parent\_message\.type | string | 
action\_result\.data\.\*\.\_source\.password | string | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_lowercase | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_number | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_symbol | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.has\_uppercase | boolean | 
action\_result\.data\.\*\.\_source\.password\_complexity\.length | numeric | 
action\_result\.data\.\*\.\_source\.password\_complexity\.probable\_hash\_algorithms | string | 
action\_result\.data\.\*\.\_source\.payment\_method | string | 
action\_result\.data\.\*\.\_source\.previous\_message | string | 
action\_result\.data\.\*\.\_source\.prices\.\*\.currency\.abbreviation | string | 
action\_result\.data\.\*\.\_source\.prices\.\*\.currency\.raw | string | 
action\_result\.data\.\*\.\_source\.prices\.\*\.raw | string | 
action\_result\.data\.\*\.\_source\.prices\.\*\.value | numeric | 
action\_result\.data\.\*\.\_source\.quantity\.available\.raw | string | 
action\_result\.data\.\*\.\_source\.quantity\.sold\.raw | string | 
action\_result\.data\.\*\.\_source\.raw\_href | string |  `url` 
action\_result\.data\.\*\.\_source\.reputation\.number\_of\_downvotes | numeric | 
action\_result\.data\.\*\.\_source\.reputation\.number\_of\_upvotes | numeric | 
action\_result\.data\.\*\.\_source\.reputation\.score | string | 
action\_result\.data\.\*\.\_source\.resource\_fpid | string | 
action\_result\.data\.\*\.\_source\.room\_count\.count | numeric | 
action\_result\.data\.\*\.\_source\.service\_code | numeric | 
action\_result\.data\.\*\.\_source\.sharing\_group\_id | string | 
action\_result\.data\.\*\.\_source\.shipping\.\*\.raw | string | 
action\_result\.data\.\*\.\_source\.ships\_from | string | 
action\_result\.data\.\*\.\_source\.ships\_to | string | 
action\_result\.data\.\*\.\_source\.site\.base\_uris | string |  `url` 
action\_result\.data\.\*\.\_source\.site\.basetypes | string | 
action\_result\.data\.\*\.\_source\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.site\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.site\.title | string | 
action\_result\.data\.\*\.\_source\.site\.type | string | 
action\_result\.data\.\*\.\_source\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.\_header\.collected\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.\_header\.observed\_at | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.activity\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.activity\.type | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.avatar\_uri\.href | string |  `url` 
action\_result\.data\.\*\.\_source\.site\_actor\.avatar\_url | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.basetypes | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.body\.enrichments\.links\.\*\.href | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.body\.text/html\+sanitized | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.bot | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.comment\_reputation\.number\_of\_upvotes | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.description | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.discriminator | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.enrichments\.links\.\*\.href | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.first\_name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.first\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.first\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.first\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.flair\.flair\_text | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_admin | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_donor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_investor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_premium | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_private | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_pro | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.is\_verified | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.joined\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.joined\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.joined\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_active\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_active\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_active\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.legacy\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.names\.aliases | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.native\_id | string |  `url`  `md5` 
action\_result\.data\.\*\.\_source\.site\_actor\.nick | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.number\_following | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.number\_of\_followers | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.number\_of\_messages | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.pgp\_key\_public | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.post\_reputation\.number\_of\_upvotes | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.reputation\.count\_ratings | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.reputation\.negative\_feedback | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.reputation\.positive\_feedback | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.reputation\.score | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.reputation\.site\_actor\_rating | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.roles\.\*\.id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.roles\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.sales\.total\_transactions | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.icon\_url | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.last\_observed\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.last\_observed\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.last\_observed\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.native\_id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.region | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.server\_owner\.id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.server\_owner\.username | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.site\.is\_deleted | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.site\.title | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.title | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.server\.verification\_level | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.\_header\.collected\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.\_header\.observed\_at | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.avatar\_uri\.href | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.body\.text/html\+sanitized | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.is\_donor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.is\_investor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.is\_premium | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.is\_private | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.is\_pro | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.is\_verified | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.number\_following | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.number\_of\_followers | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.number\_of\_messages | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.title | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.\_header\.collected\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.\_header\.observed\_at | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.avatar\_uri\.href | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.body\.text/html\+sanitized | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.is\_donor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.is\_investor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.is\_premium | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.is\_private | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.is\_pro | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.is\_verified | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.number\_following | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.number\_of\_followers | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.number\_of\_messages | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.title | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.\_header\.collected\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.\_header\.observed\_at | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.avatar\_uri\.href | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.text/html\+sanitized | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_donor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_investor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_premium | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_private | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_pro | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_verified | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.number\_following | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.number\_of\_followers | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.number\_of\_messages | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.description\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.site\_type | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.tags\.\*\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.tags\.\*\.parent\_tag\.name | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.title | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.\_header\.collected\_fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.\_header\.observed\_at | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.avatar\_uri\.href | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.enrichments\.language | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.text/html\+sanitized | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.body\.text/plain | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.created\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.created\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.created\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_donor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_investor | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_premium | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_private | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_pro | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.is\_verified | boolean | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.native\_id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.number\_following | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.number\_of\_followers | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.number\_of\_messages | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.site\_actor\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.site\_actor\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.site\_actor\.source\_uri | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.source\_uri | string |  `url` 
action\_result\.data\.\*\.\_source\.site\_actor\.title | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.type | string | 
action\_result\.data\.\*\.\_source\.site\_actor\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.site\_actor\.username | string |  `url`  `user name` 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.count | numeric | 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.first\_resource\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.first\_resource\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.first\_resource\.native\_id | string | 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.last\_resource\.fpid | string | 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.last\_resource\.names\.handle | string | 
action\_result\.data\.\*\.\_source\.site\_actor\_count\.last\_resource\.native\_id | string | 
action\_result\.data\.\*\.\_source\.size\.number\_of\_bytes | numeric | 
action\_result\.data\.\*\.\_source\.size\.raw | string | 
action\_result\.data\.\*\.\_source\.source | string |  `url` 
action\_result\.data\.\*\.\_source\.source\_type | string | 
action\_result\.data\.\*\.\_source\.source\_uri | string |  `url` 
action\_result\.data\.\*\.\_source\.syntax | string | 
action\_result\.data\.\*\.\_source\.thread\.native\_id | string | 
action\_result\.data\.\*\.\_source\.thread\.site\.behavior | string | 
action\_result\.data\.\*\.\_source\.thread\.site\.href | string | 
action\_result\.data\.\*\.\_source\.thread\.site\.target | string | 
action\_result\.data\.\*\.\_source\.thread\.type | string | 
action\_result\.data\.\*\.\_source\.thread\_count\.count | numeric | 
action\_result\.data\.\*\.\_source\.times\_seen | numeric | 
action\_result\.data\.\*\.\_source\.timestamp | string | 
action\_result\.data\.\*\.\_source\.title | string | 
action\_result\.data\.\*\.\_source\.to\_ids | boolean | 
action\_result\.data\.\*\.\_source\.top\_domains\.\*\.count | numeric | 
action\_result\.data\.\*\.\_source\.top\_domains\.\*\.value | string | 
action\_result\.data\.\*\.\_source\.top\_domains\.count | numeric | 
action\_result\.data\.\*\.\_source\.top\_domains\.value | string | 
action\_result\.data\.\*\.\_source\.top\_passwords\.\*\.count | numeric | 
action\_result\.data\.\*\.\_source\.top\_passwords\.\*\.value | string |  `sha1`  `email`  `md5` 
action\_result\.data\.\*\.\_source\.total\_records | numeric | 
action\_result\.data\.\*\.\_source\.track1 | string | 
action\_result\.data\.\*\.\_source\.track2 | string | 
action\_result\.data\.\*\.\_source\.track\_information | string | 
action\_result\.data\.\*\.\_source\.type | string | 
action\_result\.data\.\*\.\_source\.unique\_records | numeric | 
action\_result\.data\.\*\.\_source\.unique\_visits | numeric | 
action\_result\.data\.\*\.\_source\.updated\_at\.date\-time | string | 
action\_result\.data\.\*\.\_source\.updated\_at\.raw | string | 
action\_result\.data\.\*\.\_source\.updated\_at\.timestamp | numeric | 
action\_result\.data\.\*\.\_source\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.uuid | string | 
action\_result\.data\.\*\.\_source\.value\.attachment | string |  `sha256` 
action\_result\.data\.\*\.\_source\.value\.comment | string |  `file name` 
action\_result\.data\.\*\.\_source\.value\.domain | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.\_source\.value\.email\-src | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.\_source\.value\.ip\-dst | string |  `fp attribute value`  `ip` 
action\_result\.data\.\*\.\_source\.value\.ip\-dst\|port | string |  `fp attribute value` 
action\_result\.data\.\*\.\_source\.value\.link | string |  `url` 
action\_result\.data\.\*\.\_source\.value\.md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.\_source\.value\.other | string | 
action\_result\.data\.\*\.\_source\.value\.sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.\_source\.value\.sha256 | string |  `fp attribute value`  `sha256` 
action\_result\.data\.\*\.\_source\.value\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.\_source\.value\.x509\-fingerprint\-sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.\_type | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary\.total\_results | numeric | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'list indicators'
Fetch a list of IoCs that occur in the context of an event from the Flashpoint Platform

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**attributes\_types** |  optional  | Enable a search by attribute types\(allows Comma\-separated list\) | string |  `fp attribute type` 
**query** |  optional  | Filter the results based on the field value or free text search | string |  `fp attribute value` 
**limit** |  optional  | Maximum number of indicators to be fetched \(default\: 500\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.attributes\_types | string |  `fp attribute type` 
action\_result\.parameter\.limit | numeric | 
action\_result\.parameter\.query | string |  `fp attribute value` 
action\_result\.data\.\*\.Attribute\.Event\.RelatedEvent\.\*\.Event\.fpid | string | 
action\_result\.data\.\*\.Attribute\.Event\.RelatedEvent\.\*\.Event\.info | string | 
action\_result\.data\.\*\.Attribute\.Event\.fpid | string | 
action\_result\.data\.\*\.Attribute\.Event\.href | string | 
action\_result\.data\.\*\.Attribute\.Event\.info | string | 
action\_result\.data\.\*\.Attribute\.Event\.timestamp | string | 
action\_result\.data\.\*\.Attribute\.category | string | 
action\_result\.data\.\*\.Attribute\.fpid | string | 
action\_result\.data\.\*\.Attribute\.href | string | 
action\_result\.data\.\*\.Attribute\.timestamp | string | 
action\_result\.data\.\*\.Attribute\.type | string |  `fp attribute type` 
action\_result\.data\.\*\.Attribute\.uuid | string | 
action\_result\.data\.\*\.Attribute\.value\.comment | string | 
action\_result\.data\.\*\.Attribute\.value\.md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.authors | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.description | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.galaxy\_id | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.external\_id | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.kill\_chain | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.mitre\_data\_sources | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.mitre\_platforms | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.refs | string |  `url` 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.meta\.synonyms | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.source | string |  `url` 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.tag\_id | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.tag\_name | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.type | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.uuid | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.value | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.GalaxyCluster\.\*\.version | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.description | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.icon | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.name | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.namespace | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.type | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.uuid | string | 
action\_result\.data\.\*\.Event\.Galaxy\.\*\.version | string | 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.date | string | 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.fpid | string | 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.info | string | 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.timestamp | string | 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.uuid | string | 
action\_result\.data\.\*\.Event\.Tag\.\*\.name | string |  `file name` 
action\_result\.data\.\*\.Event\.Tag\.\*\.numerical\_value | string | 
action\_result\.data\.\*\.Event\.Tags | string | 
action\_result\.data\.\*\.Event\.attack\_ids | string | 
action\_result\.data\.\*\.Event\.attribute\_count | string | 
action\_result\.data\.\*\.Event\.date | string | 
action\_result\.data\.\*\.Event\.event\_creator\_email | string |  `email` 
action\_result\.data\.\*\.Event\.fpid | string | 
action\_result\.data\.\*\.Event\.href | string |  `url` 
action\_result\.data\.\*\.Event\.info | string | 
action\_result\.data\.\*\.Event\.publish\_timestamp | string | 
action\_result\.data\.\*\.Event\.report | string |  `url` 
action\_result\.data\.\*\.Event\.reports | string |  `url` 
action\_result\.data\.\*\.Event\.timestamp | string | 
action\_result\.data\.\*\.Event\.uuid | string | 
action\_result\.data\.\*\.basetypes | string |  `fp query basetypes` 
action\_result\.data\.\*\.category | string | 
action\_result\.data\.\*\.fpid | string | 
action\_result\.data\.\*\.header\_\.indexed\_at | numeric | 
action\_result\.data\.\*\.header\_\.ingested\_at | numeric | 
action\_result\.data\.\*\.header\_\.is\_visible | boolean | 
action\_result\.data\.\*\.header\_\.observed\_at | numeric | 
action\_result\.data\.\*\.header\_\.source | string | 
action\_result\.data\.\*\.href | string |  `url` 
action\_result\.data\.\*\.timestamp | string | 
action\_result\.data\.\*\.type | string |  `fp attribute type` 
action\_result\.data\.\*\.uuid | string | 
action\_result\.data\.\*\.value\.AS | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.attachment | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.authentihash | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.btc | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.campaign\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.comment | string |  `url` 
action\_result\.data\.\*\.value\.cookie | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.domain | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.value\.email\-dst | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.value\.email\-src | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.value\.email\-src\-display\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.email\-subject | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.filename | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.first\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.float | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.github\-username | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.hostname | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.imphash | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.ip\-dst | string |  `fp attribute value`  `ip` 
action\_result\.data\.\*\.value\.ip\-dst\|port | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.ip\-src | string |  `fp attribute value`  `ip` 
action\_result\.data\.\*\.value\.link | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.malware\-sample | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.value\.mutex | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.other | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.pattern\-in\-file | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.pattern\-in\-memory | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.pdb | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.port | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.regkey | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.regkey\|value | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.value\.sha256 | string |  `fp attribute value`  `sha256` 
action\_result\.data\.\*\.value\.sha512 | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.size\-in\-bytes | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.snort | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.ssdeep | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.target\-external | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.target\-machine | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.target\-org | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.text | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.threat\-actor | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.twitter\-id | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.uri | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.value\.user\-agent | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-creation\-date | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-registrant\-email | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.value\.whois\-registrant\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-registrant\-phone | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-registrar | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.x509\-fingerprint\-md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.value\.x509\-fingerprint\-sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.value\.x509\-fingerprint\-sha256 | string |  `fp attribute value`  `sha256` 
action\_result\.data\.\*\.value\.yara | string |  `fp attribute value` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary\.total\_iocs | numeric | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'search indicators'
Fetch an IoC value of a specific attribute type from the list of available IoCs on the Flashpoint Platform

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**attribute\_type** |  required  | Retrieve specific indicator's attribute type result | string |  `fp attribute type` 
**attribute\_value** |  required  | Retrieve specific indicator's attribute type result based on the provided value | string |  `fp attribute value` 
**limit** |  optional  | Maximum number of reports to be fetched \(default\: 500\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.attribute\_type | string |  `fp attribute type` 
action\_result\.parameter\.attribute\_value | string |  `fp attribute value` 
action\_result\.parameter\.limit | numeric | 
action\_result\.data\.\*\.Attribute\.Event\.RelatedEvent\.\*\.Event\.fpid | string | 
action\_result\.data\.\*\.Attribute\.Event\.RelatedEvent\.\*\.Event\.info | string | 
action\_result\.data\.\*\.Attribute\.Event\.fpid | string | 
action\_result\.data\.\*\.Attribute\.Event\.href | string | 
action\_result\.data\.\*\.Attribute\.Event\.info | string | 
action\_result\.data\.\*\.Attribute\.Event\.timestamp | string | 
action\_result\.data\.\*\.Attribute\.category | string | 
action\_result\.data\.\*\.Attribute\.fpid | string | 
action\_result\.data\.\*\.Attribute\.href | string | 
action\_result\.data\.\*\.Attribute\.timestamp | string | 
action\_result\.data\.\*\.Attribute\.type | string |  `fp attribute type` 
action\_result\.data\.\*\.Attribute\.uuid | string | 
action\_result\.data\.\*\.Attribute\.value\.comment | string | 
action\_result\.data\.\*\.Attribute\.value\.md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.fpid | string | 
action\_result\.data\.\*\.Event\.RelatedEvent\.\*\.Event\.info | string | 
action\_result\.data\.\*\.Event\.Tags | string | 
action\_result\.data\.\*\.Event\.fpid | string | 
action\_result\.data\.\*\.Event\.href | string |  `url` 
action\_result\.data\.\*\.Event\.info | string | 
action\_result\.data\.\*\.Event\.timestamp | string | 
action\_result\.data\.\*\.category | string | 
action\_result\.data\.\*\.fpid | string | 
action\_result\.data\.\*\.href | string |  `url` 
action\_result\.data\.\*\.timestamp | string | 
action\_result\.data\.\*\.type | string |  `fp attribute type` 
action\_result\.data\.\*\.uuid | string | 
action\_result\.data\.\*\.value\.AS | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.attachment | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.authentihash | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.btc | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.campaign\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.comment | string | 
action\_result\.data\.\*\.value\.cookie | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.domain | string |  `fp attribute value`  `domain` 
action\_result\.data\.\*\.value\.email\-dst | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.value\.email\-src | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.value\.email\-src\-display\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.email\-subject | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.filename | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.first\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.float | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.github\-username | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.hostname | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.imphash | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.ip\-dst | string |  `fp attribute value`  `ip` 
action\_result\.data\.\*\.value\.ip\-dst\|port | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.ip\-src | string |  `fp attribute value`  `ip` 
action\_result\.data\.\*\.value\.link | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.malware\-sample | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.value\.mutex | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.other | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.pattern\-in\-file | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.pattern\-in\-memory | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.pdb | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.port | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.regkey | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.regkey\|value | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.value\.sha256 | string |  `fp attribute value`  `sha256` 
action\_result\.data\.\*\.value\.sha512 | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.size\-in\-bytes | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.snort | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.ssdeep | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.target\-external | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.target\-machine | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.target\-org | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.text | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.threat\-actor | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.twitter\-id | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.uri | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.url | string |  `fp attribute value`  `url` 
action\_result\.data\.\*\.value\.user\-agent | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-creation\-date | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-registrant\-email | string |  `fp attribute value`  `email` 
action\_result\.data\.\*\.value\.whois\-registrant\-name | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-registrant\-phone | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.whois\-registrar | string |  `fp attribute value` 
action\_result\.data\.\*\.value\.x509\-fingerprint\-md5 | string |  `fp attribute value`  `md5` 
action\_result\.data\.\*\.value\.x509\-fingerprint\-sha1 | string |  `fp attribute value`  `sha1` 
action\_result\.data\.\*\.value\.x509\-fingerprint\-sha256 | string |  `fp attribute value`  `sha256` 
action\_result\.data\.\*\.value\.yara | string |  `fp attribute value` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary\.total\_iocs | numeric | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 