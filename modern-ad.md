# Modern Active Directory Notes

## Introduction

[Modern Active Directory](https://github.com/dakhama-mehdi/Modern_ActiveDirectory) creates an HTML report of a given AD domain and provides graphical elements to aid in visualization and reporting.  

## Setup & Report Generation

1. Open an administrator powershell session
2. `Install-Module -Name ModernActiveDirectory`
3. Accept any confirmation prompts
4. After the module is installed, close the powershell session
5. Open a new administrator powershell session
6. `Get-ADModernReport -illimitedsearch -SavePath C:\`
   - illimited search overides the 300 object limit
   - SavePath targets the location
   - Report will open automatically in your default browser

## Using the Report

![AD Dashboard](https://github.com/engineeringpenguins/minor-projects/blob/main/images/modernad/modernad-dash.png)  

Useage is even simpler than installation for this application. The dashboard that you are presented with on opening the report is a static page with little to no interaction built in. You can drill into the data using the dropdowns in the upper left to filter for specific attributes. The output of the report and layout/presentation does not appear to be customizeable.

## Thoughts

Cool idea and extremely simple to setup and use. It's dissapointing that it lacks most functionality that would make it useful for an administrator, but it provides a great starting point for tier 1 support or management to gain visability into the AD enviornment. I would not reccomend this tool for anyone working on the domain or for anything requiring visability. The strength of modern AD seems to be in its simple nature and presentation. Running this on a daily schedule and having it sent to staff that want a highlevel summary of the state of AD is how I believe modern AD should be used.  