---
layout: post
title:  "Salesforce CLI CheatSheet"
date:   2023-04-19 20:15:00 +0800
categories: 教程之CheatSheet
---

# Salesforce CLI CheatSheet

```sh
# Login DevHub, and Create a ScratchOrg of it
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

# Scratch Org
# - https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs_def_file.htm
sfdx force:org:create -f config/project-scratch-def.json --setalias lwcRecipesScratch --durationdays 7 --setdefaultusername
```

