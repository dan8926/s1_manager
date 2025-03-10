<div id="top"></div>

<h1 align="center">S1 Manager</h1>

<div align="center">

![S1 Manager][product-screenshot]

</div>

The S1 Manager tool is a GUI-based application to assist SentinelOne administrators in performing specific tasks via the v2.1 API. 

> Note: This tool requires a SentinelOne Management Console and an API Token for a user with appropriate permissions to run the various API calls. 

> Important: This tool is provided "As Is" and comes with no warranty, guarantee, or support. Use of this tool assumes the user has an active license to use the SentinelOne product and has reviewed the code to understand what actions the tool is performing.

<div align="center">

[Report a Bug or Make a Feature Request][issues-url]

</div>

<br />

<!-- GETTING STARTED -->
## Getting Started

### Download EXE
To download the latest EXE build:
- [https://github.com/DylanCS1/s1_manager/releases/download/v2022.2.4/s1_manager-2022.2.4.exe](https://github.com/DylanCS1/s1_manager/releases/download/v2022.2.4/s1_manager-2022.2.4.exe)
- SHA1: 4D12F2083D59E0E944268C0187F8C118BA39E5A3

To download the pre-2022 release:
- [https://github.com/DylanCS1/s1_manager/raw/main/.COMPILED/s1_manager-1.0.exe](https://github.com/DylanCS1/s1_manager/raw/main/.COMPILED/s1_manager-1.0.exe)
- SHA1: 1E03D09572BFAA5823295606DDE1D39A94EB6939

> Note: The tool is currently developed in Python 3.10, and tested on Windows 10 x64. Although it can feasibly work on Linux/macOS it is not fully tested on those operating systems at this time.

## Windows
### Run from source
To get a local copy up and running follow these simple steps:
1. Clone the repo
   ```sh
   git clone https://github.com/DylanCS1/s1_manager.git
   ```
2. Install Python package dependencies
   ```sh
   pip install install -r requirements.txt
   ```
3. Run the s1_manager.py
   ```sh
   python3 s1_manager.py
   ```

### Build EXE for Windows
1. Clone the repo
   ```sh
   git clone https://github.com/DylanCS1/s1_manager.git
   ```
2. Install Python package dependencies
   ```sh
   pip install install -r requirements.txt
   ```
3. Build EXE with pyinstaller
   ```sh
   pyinstaller s1_manager.spec
   ```


## Linux
This script relies on files within the /theme and /ico folders, which are hard coded. If you opt to move them make sure to update the `logo` and `windows.tk.call` variables in s1_manager.py.

1. Clone the repo
   ```sh
   git clone https://github.com/DylanCS1/s1_manager.git
   ```
2. Install Python package dependencies
   ```sh
   sudo pip3 install -r requirements.txt
   ```
3. You may need to install Tkinter separately. Refer to the tkdocs for details: https://tkdocs.com/tutorial/install.html#install-x11-python & https://stackoverflow.com/questions/40588444/how-to-install-python3-tk-in-centos  
   Ubuntu:
   ```sh
   sudo apt install python3-tk
   ```
   CentOS:
   ```sh
   sudo yum install python3-tkinter
   ```
4. Run the s1_manager.py
   ```sh
   python3 s1_manager.py
   ```

## macOS
This script relies on files within the /theme and /ico folders, which are hard coded. If you opt to move them make sure to update the `logo` and `windows.tk.call` variables in s1_manager.py.
> Note: Requires Python 3.10+

1. Clone the repo
   ```sh
   git clone https://github.com/DylanCS1/s1_manager.git
   ```
2. Install Python package dependencies
   ```sh
   sudo pip3 install -r requirements.txt
   ```
4. Run the s1_manager.py
   ```sh
   python3 s1_manager.py
   ```


<p align="right">(<a href="#top">back to top</a>)</p>


<!-- USAGE EXAMPLES -->
## Usage

Presently, everything in the S1 Manager tool runs on a single thread so when executing a task the GUI will appear to be "dead" (GUI cannot accept new events). You will just need to be patient :)  

> The permissions assigned to the user associated with the API Token define what actions can be performed, and at what scope.

### Login:

1. Input your **SentinelOne Management Console** address (e.g., https://abc-corp.sentinelone.net)
2. Input your user account **API Token**
3. Add proxy address details (if needed) 
4. If using an On-Prem console with a self-signed certificate you will need to uncheck the **Use SSL** option
5. Click **Submit**

![Login][login-view]

## Available Export Operations

### Export Deep Visibility Events

Export events from Deep Visibility to an XLSX based on a Deep Visibility Query ID. Multiple datapoints are temporarily written to CSVs which then get combined into a single XLSX, one CSV per worksheet in the XLSX.

To generate a Deep Visibility query:
1. Log in to the Management Console
2. Go to the Deep Visibility Page and create the query. For example: `EndpointName Contains Anycase "win10" AND EndpointOS = "windows"`
 
![Deep Visibility Query][dv-screenshot]


3. Open your web browser's **Developer Tools** (```F12 or CTRL+SHIFT+i```)
4. Open the **Network** tab 
5. Run the query in the Management Console
6. Click on **'query-streaming-status'**, **'count-by-type'**, or **'all-events'**
7. Open the Payload view to find and copy your **queryID**

![Developer Tools example][dev-tools-screenshot]



### Export Activity Log
**Deprecated** - Feature resides in Console

Search and Export the Activity log.
> Currently, the exported results are constrained by the FROM and TO dates, not the search term. To see search results more clearly, refer to the s1_manager.log 


> This can take a very long time depending on the number of events to fetch. If 10,000 or fewer entries are needed, it is recommended instead to export to CSV from the Management Console as that is much faster.

Process:
1. Input a **FROM** and **TO** date in the format of *yyyy-mm-dd*
2. Input a search term (string). **Note:** Search is not Case Sensitive.  
3. Click **Search** to see filtered results
4. Click **Export** to save all Activity results for the given timeframe to CSV



### Export Endpoints
**Deprecated** - Feature resides in Console

Export Endpoint Light-report to CSV and convert to XLSX.
> This includes up to 300,000 endpoints and associated details.

**Note:** The previous method used by this operation was inefficient and for very large numbers of endpoints could take hours. The new method relies on the Light Report export option added in Rio GA.


### Export Exclusions

Export all exclusions. The scope of entries is associated with the API Token and its level of access.

> This operation creates one CSV per Exclusion type (file type, path, browser, certificate, and hash). These are then merged into a single XLSX.


![Exclusion CSV Example][exclusion-screenshot]



### Export Endpoint Tags

Export Endpoint Tag details to CSV for all scopes in Management Console.


### Export Local Config

Export Agent local configuration(s) to a single JSON file for all Agent UUIDs in a supplied CSV. This can be useful to determine what local configuration is applied to agent, which may not be easily identified via the Management Console.

Process:
1. Select a CSV file containing a single column of agent UUIDs


### Export Users and Roles
**Deprecated** - Feature resides in Console

Export Management Console user or role details to a CSV or XLSX file.


### Export Ranger Inventory

Export Ranger Inventory details to CSV.

> If processing multiple Accounts or Sites, one CSV per ID will be created

Process:
1. Select which scope to export Ranger Inventory from: **Account** or **Site**
2. Select a CSV containing a single column of Account or Site IDs to process
3. Pick a time period for data export


### Export Blacklist

Export all blacklist entries. The scope of entries is associated with the API Token and its level of access.

> This creates a CSV and an XLSX


<p align="right">(<a href="#top">back to top</a>)</p>


## Available Manage Operations

### Upgrade Agents
**Deprecated** - Feature resides in Console

Bulk upgrade agents from a named endpoint list in a CSV file.

Requirements:
- A CSV file containing a single column of Endpoint names to be upgraded
- All endpoints should have unique names to avoid affecting duplicate entries
> Refer to the SentinelOne KB on [Creating Filters for Endpoints](https://support.sentinelone.com/hc/en-us/articles/360004221853-Creating-Filters-for-Endpoints-Multi-Site-) for more information.

Process:
1. Export the Packages List and get the relevant Package ID
> If you are using Microsoft Excel, make sure the ID cell is formatted as Text when imported, otherwise, some of the digits might be changed to zeros
> [https://support.microsoft.com/en-us/help/269370/last-digits-are-changed-to-zeroes-when-you-type-long-numbers-in-cells](https://support.microsoft.com/en-us/help/269370/last-digits-are-changed-to-zeroes-when-you-type-long-numbers-in-cells)
2. Insert the package ID to use for upgrade.
3. Select a CSV containing a single column of endpoint names to be upgraded.
4. Toggle the 'Use Schedule' switch on if you want the upgrade to occur per the defined schedule in the Console.

Example of CSV:  
![Endpoint Names Example][endpoint-screenshot]  



### Move Agents

Move the agents listed in the CSV to the target site ID and target group ID.
> If the target group is dynamic the agent will only be moved into the parent site scope. It is expected to see an 'Error code: 409' for this situation.

Requirements:
- A CSV file containing the Endpoint names to be moved
> There should be no column headers and the columns should consist of endpoint name, target group ID, and target site ID
- All endpoints should have unique names to avoid affecting duplicate entries
> Refer to the SentinelOne KB on [Creating Filters for Endpoints](https://support.sentinelone.com/hc/en-us/articles/360004221853-Creating-Filters-for-Endpoints-Multi-Site-) for more information.


Process:
1. Export groups list to get the relevant Group ID 
> Please see the note above if using Microsoft Excel 

![Group ID example][group-id-screenshot]  
2. Create a CSV file containing three columns without headers (refer to requirements above)  
![Example CSV][csv-example-screenshot]



### Assign Customer Identifier
**Deprecated** - Feature resides in Console

Easily add a Customer Identifier to Agents from a source CSV of endpoint names.

Requirements:
- A named list of endpoints who share a similar logical trait (i.e they are all Dev Servers)
> Refer to the SentinelOne KB on [Creating a User Defined Endpoint ID](https://support.sentinelone.com/hc/en-us/articles/360038970994-Creating-a-User-Defined-Endpoint-ID)


Process:
1. Insert the Customer Identifier
2. Select a CSV containing endpoint names
> If you have duplicate names, all the endpoints with this name will be assigned the same customer identifier

![Endpoint Names Example][endpoint-screenshot]



### Decommission Agents

Decommission SentinelOne agents in bulk using a source CSV of Endpoint names.

Requirements:
- A CSV containing the list of endpoints that need to be decomissioned
> Refer to the SentinelOne KB on [Removing an Agent from the Console](https://support.sentinelone.com/hc/en-us/articles/360004242793-Removing-an-Agent-from-the-Console-Decommission-Multi-Site-)

Process:
1. Select a CSV containing endpoint names to be decomissioned
> If you have duplicate names, all the endpoints with this name will be decomissioned.

![Endpoint Names Example][endpoint-screenshot]



### Manage Endpoint Tags

Add or Remove Endpoint Tags from Agents.

Process:
1. Select an action: **Add** or **Remove**
2. Input the **Endpoint Tag ID** to add/remove
3. Select the **Agent Identifier Type** used in your source CSV (Agent UUID or Endpoint Name)
4. Select a CSV file containing a single column of agent UUIDs or endpoint names (this should align with your selection in step 3)



### Bulk Resolve Threats

Adds a predefined note and sets the selected Analyst Verdict on a large group of threats (incidents) that match the searched value, then closes the incidents as Resolved.

Process:
1. Select incident search type: **Threat Name** or **SHA1**
2. Input an appropriate search string based on the choice made above
   - *Threat Name* = A partial or complete search string. Search is not case-sensitive, and multiple words should not be enclosed in quotes.
      > May have unexpected results with special characters.
   - *SHA1* = One or more comma-separated SHA1s (do not include any whitespace)
3. Select the **Analyst Verdict** from the drop-down: *undefined*, *suspicious*, *false_positive*, or *true_positive*
4. Input one or more **Site IDs**, separated by a comma (do not include spaces)



### Bulk Enable Agents

Send 'Enable Agent' action to all agents that are disabled in one or more Groups. 

> **Note:** This does not send a reboot request. This is intentional to avoid an accidental forcecd reboot of a large number of endpoints.

Process:
1. Input one or more group IDs to send **Enable Agent** action to
   - *Group IDs* should be comma-separated without any whitespace characters



### Update System Configuration

**Note:** This can cause unexpected results. Use with caution.

Accepts a JSON file with the changes to apply to one, or more, Site or Account IDs. 

Process:
1. Select whether you are updating one or more Sites or Accounts
2. Input one, or more, IDs of the type chosen above. *Multiple IDs should be comma-separated with no white space.*
3. Click browse to select a JSON file with the new configuration to apply (see below for JSON example)

Example JSON:  
```json
{
	"data": {
		"advancedMode": "true",
		"uiInactivityTimeoutSeconds": 7200,
		"rememberMeLength": 1440,
		"globalTwoFaEnabled": "true",
		"cloudIntelligenceOn": "true"
	},
	"filter": {}
}
```

> Not all of the available "data" values are shown above. Refer to the API Docs for a full list of available parameters.  

**Notes:**
- In the tool's current form, it relies on the "filter" subsection of the JSON - do not remove it.  


### Import Blacklist

Process:
1. Select scope (group, site, or account).
   - Note: Tenant/Global not supported at this time.
2. Input one, or more, IDs of the chose scope type. *Multiple IDs should be comma-separated with no white space.*
3. Click browse to select a CSV with the blacklist entries to import.

CSV requirements:
- The first row is ignored by the script, this row can include headers or be empty
- The first column must contain the SHA1 value
> 2022.2.3 strips out whitespace from this column
> 2022.2.4 checks length. If hash is not 40 characters exactly returns an error for that hash.
- The second column must contain the OS Type (windows, linux, macos, windows_legacy)
- The third column optionally can contain a description

Refer to the following screenshot for an example.  

![Blacklist CSV Example][blacklist-screenshot]


### Import Exclusion

Process:
1. Select scope (group, site, or account).
   - Note: Tenant/Global not supported at this time.
2. Input one, or more, IDs of the chose scope type. *Multiple IDs should be comma-separated with no white space.*
3. Click browse to select a CSV with the exclusion entries to import.

CSV requirements:
- The first row is ignored by the script, this row can include headers or be empty
- The first column must contain the value to exclude
- The second column must contain the type of exclusion 
- The third column must contain the OS type
- The fourth column is applicable only to the `path` type, and must contain the mode
- The fifth column is applicable only to the `path` type, and must contain the pathExclusionType
- The sixth column optionally can contain a description

Available exclusion types:
- white_hash
- path
- file_type
- certificate
- browser

Available OS Types:
- windows
- windows_legacy
- macos
- linux

Available modes for path exclusions:
- suppress == Suppress Alerts - All engines
- suppress_dfi_only == Suppress Alerts - Static AI only
- suppress_dynamic_only == Suppress Alerts - Dynamic AI only
- suppress_app_control == Suppress Alerts - App Control only
- disable_all_monitors == Performance Focus
- disable_all_monitors_deep == Performance Focus extended
- disable_in_process_monitor == Interoperability
- disable_in_process_monitor_deep == Interoperability extended


Refer to the following screenshot for an example.  

![Exclusion Import CSV Example][exclusion2-screenshot]


<p align="right">(<a href="#top">back to top</a>)</p>


<!-- CONTRIBUTING -->
## Contributing

Contributions are greatly appreciated.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement". Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/NewFeature`)
3. Commit your Changes (`git commit -m 'Add some NewFeature'`)
4. Push to the Branch (`git push origin feature/NewFeature`)
5. Open a Pull Request

The s1_manager tool should be run through the Python Black code formatter. Reference their documentation for more details: [Black](https://black.readthedocs.io/en/stable/)


<!-- ISSUES -->
## Reporting Issues

If you observe any issues using the S1 Manager tool, please check if this issue is already documented by checking the [issues][issues-url] page. If not, then fill out a new issue providing as much detail as possible including any inputs, observed behavior/errors in the UI, etc. Additionally, if you launch the S1 Manager tool with `--debug` argument, verbose logging is generated which may assist in troubleshooting. If you can easily replicate the issue please do so with debug logging enabled and provide the log file.  
> **Important Note:** The debug logging is quite verbose and can include tens of thousands of lines. Additionally, your API Token will be displayed in plaintext so this should not be used except for troubleshooting. When done, the **s1_manager_debug** log should be properly purged from your file system.

```sh
python s1_manager.py --debug
```
OR
```sh
s1_manager.exe --debug
```


<!-- LICENSE -->
## License

Distributed under the MIT License. See [LICENSE.txt][license-url] for more information.

SentinelOne and the SentinelOne logomark are &trade; of [SentinelOne](https://www.sentinelone.com/legal/tm-guidelines/ "SentinelOne Trademark").


<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements

A huge thank you to the following individuals for starting the S1 Manager tool project:
- [guysentinel](https://github.com/guysentinel "guysentinel")
- [tomerbsentinel](https://github.com/tomerbsentinel "tomerbsentinel")
- [RokoS1](https://github.com/RokoS1 "RokoS1")

[Click to see all Contributors][contributors-url]

And to the following resources:
- [SentinelOne](https://www.sentinelone.com/ "SentinelOne")
- [Python](https://www.python.org/downloads/ "Python Download")
- [Requests](https://docs.python-requests.org/en/latest/ "Python Requests library")
- [Babel](https://babel.pocoo.org/en/latest/index.html "Python Babel library")
- [pandas](https://pandas.pydata.org/ "Python pandas")
- [Black](https://github.com/psf/black "Python Black code formatter")
- [Othneildrew](https://github.com/othneildrew) for the README.md template. 
- [rdbende Forest-ttk-theme](https://github.com/rdbende/Forest-ttk-theme)

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- LINKS -->
[contributors-url]: https://github.com/DylanCS1/s1_manager/graphs/contributors "Contributors"
[issues-url]: https://github.com/DylanCS1/s1_manager/issues "Issues"
[license-url]: https://github.com/DylanCS1/s1_manager/blob/main/LICENSE.txt "MIT License"

<!-- Images -->
[product-screenshot]: readme/product_screenshot.png "S1 Manager Screenshot"
[login-view]: readme/login_view.png "Login"
[dv-screenshot]: readme/dv_query.png "Deep Visibility Query"
[exclusion-screenshot]: readme/exclusion_export.png "Example Exclusion CSV"
[exclusion2-screenshot]: readme/wl_csv_example.png "Example Exclusion Import CSV"
[endpoint-screenshot]: readme/endpoint_names.png "CSV Endpoint Names example"
[dev-tools-screenshot]: readme/dev_tools.png "Dev Tools example"
[group-id-screenshot]: readme/group_id.png "Group ID example"
[csv-example-screenshot]: readme/csv_example.png "CSV example"
[blacklist-screenshot]: readme/bl_csv_example.png "Example Blacklist CSV"
