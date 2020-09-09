# SDL continuous integration strategy

SDL continuously checks code:
 - check style
 - static code analysis
 - Build and unit tests
 - Smoke automates tests
 - Regression tests

CI strategy described in details in [proposal](https://github.com/smartdevicelink/sdl_evolution/blob/master/proposals/0277-Continuous-Integration-And-Testing.md)

## Develop nightly and push checks

Only TCP transport is checked on 3 policy flows. 

[Develop](https://github.com/smartdevicelink/sdl_core/tree/develop) push and nightly builds are available in [view](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly/) :

 - [Code style check](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_Checkstyle/) : Use [check_style.sh](https://github.com/smartdevicelink/sdl_core/blob/master/tools/infrastructure/check_style.sh) to check sdl_core code for compilence to Google coding style.
 - Develop builds without unit tests: [Proprietary](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_NoUT_P/), [External proprietary](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_NoUT_E/), [HTTP](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_NoUT_H/) policy flows. 
- Develop builds with unit tests: [Proprietary](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_UT_P/), [External proprietary](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_UT_E/), [HTTP](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20builds/job/Develop_SDL_UT_H/) policy flows. 
 - [Develop_=RUN_PUSH_AND_NIGHTLY=](
https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly/job/Develop_=RUN_PUSH_AND_NIGHTLY=/) job is trigger for listed develop build jobs


Develop branch push and nightly automated scripts checks available in [view](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/) :

### Automated scripts 

Automated scripts checks triggered by no unit tests SDL build jobs. 
Each job contains a description of ATF test sets that it includes. 

### Automated smoke tests 

Contains basic checks from [smoke_tests.txt](https://github.com/smartdevicelink/sdl_atf_test_scripts/blob/master/test_sets/smoke_tests.txt)

A test set executed for SDL build in 3 policy flows: 
 - [Proprietary](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/job/Develop_TCP_ATF_Smoke_P)
 - [External](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/job/Develop_TCP_ATF_Smoke_E/)
 - [HTTP](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/job/Develop_TCP_ATF_Smoke_E/)
 
### Automated policy tests 

Contains main policy from [policies_all_flows.txt](https://github.com/smartdevicelink/sdl_atf_test_scripts/blob/master/test_sets/policies_all_flows.txt)

A test set executed for SDL build in 3 policy flows: 
 - [Proprietary](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/job/Develop_TCP_ATF_Policies_P)
 - [External](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/job/Develop_TCP_ATF_Policies_E/)
 - [HTTP](https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20push%20and%20nightly%20status/job/Develop_TCP_ATF_Policies_H)

### Automated regression

Contains all ATF scripts for all featured available in the development.
For parallel execution, checks are split to multiple jobs with make name template **Develop_TCP_ATF_VF{X}_{P,H,E}**. **X** is the number.

Full list of regression ATF jobs available on : https://opensdl-jenkins.prjdmz.luxoft.com/view/ATF_Regression_on_TCP/ 

## PR to develop checks

Pull request jobs are available in view: https://opensdl-jenkins.prjdmz.luxoft.com/view/PR_checks/ 

PR checks : 
 - code style
 - build with unit tests
 - [smoke_tests.txt](https://github.com/smartdevicelink/sdl_atf_test_scripts/blob/master/test_sets/smoke_tests.txt) trigger by build with unit tests.

## Weekly checks 

### Unit tests coverage 

Unit tests coverage check weekly in the job: 

https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20weekly/job/develop_weekly_coverage/

### Full ATF regression

On Saturday, SDL CI performs a full regression check on these following transports: TCP, WebSockets, SecureWebSockets, and all 3 policy flow. 
Weekly builds are available on following view https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20weekly/ 
Each SDL builds without unit tests triggers full ATF regression. List of triggered jobs available in each build job as Downstream projects : 
https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20weekly/job/Develop_SDL_NoUT_E_BWSS_OFF/ 

Full weekly status is available on https://opensdl-jenkins.prjdmz.luxoft.com/view/Develop%20weekly%20status/ 

## Feature checks:

For each feature before merging to develop should be created a list of jobs similar to develop to check that feature will no introduce a regression. 
There is a special job in CI [Feature job create]() that will create a list of jobs and a separate view for the feature.

Required input values for feeature job: 
 - Feture name (will be used for view title)
 - sdl_core feature branch and repository (master by default) 
 - sdl_atf feature branch and repository (master by default)
 - sdl_atf_test_scripts deature branch and repository. (master by default)
 - Feature test set (optional)
 - Additional info : evoluiton proposal, links to issues, etc ...
 
 After job execution will be created a view with the following checks:
  - 1. SDL build with unit tests on 3 policy flows (triggers: push, nightly)
  - 2. SDL build without unit tests on 3 policy flows (triggers: push, nightly)
  - 3. Smoke tests on 3 policy flows (triggers: build jobs without unit tests)
  - 4. Feature tests on 3 policy flows (triggers: build jobs without unit tests)
  - 5. Regression tests on 3 policy flows (triggers: build jobs without unit tests)

