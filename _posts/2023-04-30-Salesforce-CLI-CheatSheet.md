---
layout: post
title:  "Salesforce CLI CheatSheet"
date:   2023-04-30 01:40:00 +0800
categories: 教程之CheatSheet
---

# Salesforce CLI CheatSheet

```sh
# Login DevHub, and Create a ScratchOrg of it
# 打开 App Launcher 中的 "Active Scratch Orgs" 应用来管理 DevHub Org 中的 Scratch Org.
sfdx auth:web:login --setdefaultdevhubusername --setalias my-hub-org
sfdx force:org:create --definitionfile my-org-def.json --setdefaultusername --setalias my-scratch-org

# See Alias / Orgs / Org Site
sfdx alias:list
sfdx force:org:list
sfdx force:org:list --verbose
sfdx force:org:list --clean
sfdx force:org:open -u <username/alias>

# More About Setting Default Usernames
sfdx config:set defaultusername=test-wvkpnfm5z113@example.com
sfdx config:set defaultusername=my-scratch-org
sfdx config:set defaultdevhubusername=jdoe@mydevhub.com
sfdx config:unset defaultusername --global
sfdx config:unset defaultdevhubusername --global

# Aliasing
sfdx alias:set my-scratch-org=test-wvkpnfm5z113@example.com
sfdx alias:unset my-org

###
# Scratch org usage example with Github trailheadapps/lwc-recipes
sfdx auth:web:login -d -a myhuborg
sfdx force:org:create -s -f config/project-scratch-def.json -a lwc-recipes
# push source codes
sfdx force:source:push
# assign permissions
sfdx force:user:permset:assign -n recipes
# import sample data
sfdx force:data:tree:import -p ./data/data-plan.json
# open the scratch org
sfdx force:org:open
```

