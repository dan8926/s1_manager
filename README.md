<div id="top"></div>

<h1 align="center">S1 Manager</h1>

<div align="center">

![S1 Manager][product-screenshot]

</div>

The S1 Manager tool is a GUI-based application to assist SentinelOne administrators in performing specific tasks via the v2.1 API. The tool was developed in Python 3.6+ with TKinter.

> This tool requires a SentinelOne Management Console and an API Token for a user with appropriate permissions to run the various API calls. Use of this tool assumes the user has an active license to use the SentinelOne product.

<div align="center">

[Report a Bug or Make a Feature Request][issues-url]

</div>

<br />

<!-- GETTING STARTED -->
## Getting Started

### Download EXE
To download the latest release:
- [https://github.com/DylanCS1/s1_manager/releases/download/v2022.1.0/s1_manager-2022.1.0.exe](https://github.com/DylanCS1/s1_manager/releases/download/v2022.1.0/s1_manager-2022.1.0.exe)
- SHA1: 8FA6B0F480B95705C24427C73447846EE75E93A3

To download the pre-2022 release:
- [https://github.com/DylanCS1/s1_manager/raw/main/.COMPILED/s1_manager-1.0.exe](https://github.com/DylanCS1/s1_manager/raw/main/.COMPILED/s1_manager-1.0.exe)
- SHA1: 1E03D09572BFAA5823295606DDE1D39A94EB6939


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


### Build EXE
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

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- USAGE EXAMPLES -->
## Usage

Presently, everything in the S1 Manager tool runs on a single thread so when executing a task the GUI will appear to be "dead" (GUI cannot accept new events). You will just need to be patient :)  

> The permissions assigned to the user associated with the API Token define what actions can be performed, and at what scope.

### Login:

1. Input your SentinelOne Management Console address (e.g., https://abc-corp.sentinelone.net)
2. Input your user account API Token
3. Add proxy address details (if needed) 
4. If using an On-Prem console with a self-signed certificate you will need to uncheck the **Use SSL** option
5. Click *Submit*

## Available Export Operations

### Export Deep Visibility Events

Export events from Deep Visibility to an XLSX based on a Deep Visibility Query ID. Multiple datapoints are temporarily written to CSVs which then get combined into a single XLSX, one CSV per worksheet in the XLSX.

To generate a Deep Visibility query:
1. Log in to the Management Console
2. Go to the Deep Visibility Page and create the query. For example: *EndpointName Contains Anycase "win10" AND EndpointOS = "windows"*
 
![Deep Visibility Query][dv-screenshot]


3. Open your web browser's Developer Tools (```F12 or CTRL+SHIFT+i```)
4. Open the Network tab 
5. Run the query in the Management Console
6. Click on init-query and copy your `queryID`

![Developer Tools example][dev-tools-screenshot]

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Activity Log

Search and Export the Activity log.
> Search is not Case Sensitive
> Exported results are contrained by the FROM and TO dates, but not the search term.

Process:
1. Input a FROM and TO date in the format of yyyy-mm-dd
2. Input a search term (string)
3. Click Search to see filtered results
4. Click Export to save all results to CSV

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Endpoints

Export the list of Agents in the SentinelOne console to CSV or XLSX.

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Exclusions

Export all exclusions from the Account scope.
> CSVs are temporarily created and then merged into a single XLSX, one worksheet per exclusion type.

![Exclusion CSV Example][exclusion-screenshot]

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Endpoint Tags

Export Endpoint Tag details to CSV for all scopes in Management Console.

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Local Config

Export Agent local configuration(s) to a single JSON file for all Agent UUIDs in a supplied CSV.

Process:
1. Select a CSV file containing a single column of agent UUIDs

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Users

Export Management Console user details to a CSV or XLSX file.

<p align="right">(<a href="#top">back to top</a>)</p>


### Export Ranger Inventory

Export Ranger Inventory details to CSV.

> If processing multiple Accounts or Sites, one CSV per ID will be created

Process:
1. Select which scope to export Ranger Inventory from: Account or Site
2. Select a CSV containing a single column of Account or Site IDs to process
3. Pick a time period for data export

<p align="right">(<a href="#top">back to top</a>)</p>


## Available Manage Operations

### Upgrade Agents

Bulk upgrade agents from a named endpoint list in a CSV file.

Requirements:
- A CSV file containing a single column of Endpoint names to be upgraded
- All endpoints should have unique names to avoid affecting duplicate entries
> Refer to the SentinelOne KB on [Creating Filters for Endpoints](https://support.sentinelone.com/hc/en-us/articles/360004221853-Creating-Filters-for-Endpoints-Multi-Site-) for more information.

Process:
1. Export the Packages List and get the relevant Package ID
> If you are using Microsoft Excel, make sure the ID cell is formatted as Text when imported, otherwise, some of the digits might be changed to zeros
> [https://support.microsoft.com/en-us/help/269370/last-digits-are-changed-to-zeroes-when-you-type-long-numbers-in-cells](https://support.microsoft.com/en-us/help/269370/last-digits-are-changed-to-zeroes-when-you-type-long-numbers-in-cells)
2. Insert the package ID
3. Select a CSV containing a single column of endpoint names to be upgraded.
4. Toggle the 'Use Schedule' switch on if you want the upgrade to occur per the defined schedule in the Console.

Example of CSV:  
![Endpoint Names Example][endpoint-screenshot]  


<p align="right">(<a href="#top">back to top</a>)</p>


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

<p align="right">(<a href="#top">back to top</a>)</p>


### Assign Customer Identifier

Easily add a Customer Identifier to Agents from a source CSV of endpoint names.

Requirements:
- A named list of endpoints who share a similar logical trait (i.e they are all Dev Servers)
> Refer to the SentinelOne KB on [Creating a User Defined Endpoint ID](https://support.sentinelone.com/hc/en-us/articles/360038970994-Creating-a-User-Defined-Endpoint-ID)


Process:
1. Insert the Customer Identifier
2. Select a CSV containing endpoint names
> If you have duplicate names, all the endpoints with this name will be assigned the same customer identifier

![Endpoint Names Example][endpoint-screenshot]

<p align="right">(<a href="#top">back to top</a>)</p>


### Decommission Agents

Decommission SentinelOne agents in bulk using a source CSV of Endpoint names.

Requirements:
- A CSV containing the list of endpoints that need to be decomissioned
> Refer to the SentinelOne KB on [Removing an Agent from the Console](https://support.sentinelone.com/hc/en-us/articles/360004242793-Removing-an-Agent-from-the-Console-Decommission-Multi-Site-)

Process:
1. Select a CSV containing endpoint names to be decomissioned
> If you have duplicate names, all the endpoints with this name will be decomissioned.

![Endpoint Names Example][endpoint-screenshot]

<p align="right">(<a href="#top">back to top</a>)</p>


### Manage Endpoint Tags

Add or Remove Endpoint Tags from Agents.

Process:
1. Select an action (Add or Remove)
2. Input the Endpoint Tag ID to add/remove
3. Select the Agent Identifier Type used in your source CSV (Agent UUID or Endpoint Name)
4. Select a CSV file containing a single column of agent UUIDs or endpoint names (this should align with your selection in step 3)

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- ROADMAP -->
## Roadmap

Proposed (hopeful) changes to implement in the near future.

- [ ] Add additional features around newer API offerings
  - [x] Endpoint Tag management
  - [x] Export Ranger Inventory for Account or Site scope
  - [x] Export Agent Local Config
  - [x] Export Users
  - [ ] Reset, or Update Agent Local Config
  - [ ] Policy updates to specified scope(s)
  - [ ] Fetch agent logs
  - [ ] Export Tasks events
- [x] Update dependencies
- [ ] Functional improvements
   - [ ] Implement asyncio on more functions
   - [x] Consistent logging across all features of tool
   - [x] Implement code styling restraints (black code formatter applied)
   - [ ] Refactor/cleanup code
- [x] UI Enhancements
    - [x] Uniformity between panes, buttons, font, etc.
    - [x] Improve color theme
    - [x] New logo
    - [x] New executable icon

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

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- ISSUES -->
## Reporting Issues

If you observe any issues using the S1 Manager tool, please check if this issue is already documented by checking the [issues][issues-url] page. If not, then fill out a new issue providing as much detail as possible. Additionally, if you launch the S1 Manager tool with `--debug` argument, verbose logging is generated which may assist in troubleshooting.  
> **Important Note:** The debug logging is quite verbose and can include tens of thousands of lines. Additionally, your API Token will be displayed in plaintext so this should not be used except for troubleshooting. When done, the log should be properly purged from your file system.

```sh
python s1_manager.py --debug
```
OR
```sh
s1_manager.exe --debug
```

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- LICENSE -->
## License

Distributed under the MIT License. See [LICENSE.txt][license-url] for more information.

SentinelOne and the SentinelOne logomark are &trade; of [SentinelOne](https://www.sentinelone.com/legal/tm-guidelines/ "SentinelOne Trademark").

<p align="right">(<a href="#top">back to top</a>)</p>


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
[product-screenshot]: .README/product_screenshot.png "S1 Manager Screenshot"
[dv-screenshot]: .README/dv_query.png "Deep Visibility Query"
[exclusion-screenshot]: .README/exclusion_export.png "Example Exclusion CSV"
[endpoint-screenshot]: .README/endpoint_names.png "CSV Endpoint Names example"
[dev-tools-screenshot]: .README/dev_tools.png "Dev Tools example"
[group-id-screenshot]: .README/group_id.png "Group ID example"
[csv-example-screenshot]: .README/csv_example.png "CSV example"
