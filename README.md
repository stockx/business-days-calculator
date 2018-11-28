# business-days-calculator

Business Days Calculator

[![Build Status](https://travis-ci.org/andrejsstepanovs/business-days-calculator.png?branch=master)](https://travis-ci.org/andrejsstepanovs/business-days-calculator) [![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/andrejsstepanovs/business-days-calculator/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/andrejsstepanovs/business-days-calculator/)[![Coverage Status](https://coveralls.io/repos/andrejsstepanovs/business-days-calculator/badge.png?branch=master)](https://coveralls.io/r/andrejsstepanovs/business-days-calculator?branch=master) [![Latest Stable Version](https://poser.pugx.org/andrejsstepanovs/business-days-calculator/v/stable.png)](https://packagist.org/packages/andrejsstepanovs/business-days-calculator) [![License](https://poser.pugx.org/andrejsstepanovs/business-days-calculator/license.png)](https://packagist.org/packages/andrejsstepanovs/business-days-calculator)

## Install

* If you're using Composer to manage dependencies, you can use

```sh
composer require stockx/business-days-calculator
```

or add to your composer.json file:

```json
"require": {
    "stockx/business-days-calculator": "1.*",
}
```

## Testing and Releasing a New Version

This process takes a few steps, both in this repository and (typically) in Grails.

### Testing

**NOTE:** Actual semver numbers should not be used to in local testing.

* Create a branch and make your changes here.  For example `git checkout -b feature-name-dev` will match a `composer.json` requirement value of `dev-feature-name` in consuming projects.  
* Update the the `composer.json` in this project changing its `version` tag updated to match `dev-feature-name` (note the difference in name from the branch)
* Push the branch up to GitHub (no need to open a PR yet)
* Update `composer.json` in the grails repo to pull this version you're developing. This file is in `magento/app/code/local/Grails/composer.json`
* Run Composer locally on Grails.  From `magento/app/code/local/Grails/` run `./composer.phar update` and it should fetch the branch you're working off of as a package.

### Releasing an approved and tested version

One you're satisfied with your testing locally here's what you do to get it into prod.

* Set the `version` parameter of the `composer.json` in this project to the next semantic version of the project (we'll use `1.5.2` for our example).
* Commit this and open a PR.
* Get the PR approved and merged
* Pull the master branch locally and add a git tag should be added that prepends a `v` to that semver number just assigned.  `git tag v1.5.2`.  
* Push tags back up to github `git push --tags`
* Head over to grails and create a branch for your changes.
* Update the grails `composer.json` to the version you just pushed in `magento/app/code/local/Grails/composer.json`
* Push the branch and open a PR
* After approvals, merge the PR and follow the grails deployment process here: [https://stockx-services.atlassian.net/wiki/spaces/~admin/pages/31293441/Grails+Branching+and+Deployment+Process](https://stockx-services.atlassian.net/wiki/spaces/~admin/pages/31293441/Grails+Branching+and+Deployment+Process)

**Note:** ONLY Grails web boxes will consume the new version of this package upon deployment as `composer update` is run as part of the `bin/dep` deploy process.  It still needs to be implemented for the `deployscript` support box deploy script to run composer, so for now that will need to be done manually, SSHing into the boxes.

## Example

``` php
use \BusinessDays\Calculator;

$holidays = [
    new \DateTime('2000-12-31'),
    new \DateTime('2001-01-01')
];

$freeDays = [
    new \DateTime('2000-12-28')
];

$freeWeekDays = [
    Calculator::SATURDAY,
    Calculator::SUNDAY
];

$calculator = new Calculator();
$calculator->setStartDate(new \DateTime('2000-12-27'));
$calculator->setFreeWeekDays($freeWeekDays); // repeat every week
$calculator->setHolidays($holidays);         // repeat every year
$calculator->setFreeDays($freeDays);         // don't repeat

$calculator->addBusinessDays(3);             // add X working days

$result = $calculator->getDate();            // \DateTime
echo $result->format('Y-m-d');               // 2001-01-03
```
