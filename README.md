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

## Releasing a New Version

This process takes a few steps, both in this repository and (typically) in Grails.

Create a branch and make your changes here.  You can label that branch with a semver convention and a stability at the end.  For example `git checkout -b feature-name-dev` will match a `composer.json` requirement value of `dev-feature-name` in consuming projects.  For this to actually work the `composer.json` in this project will need to have its `version` tag updated to match `dev-feature-name`.  Then, this can be used to test with Composer locally on Grails.  **NOTE:** Actual semver numbers should not be used to in local testing.

One you're satisfied with your testing locally, set the `version` parameter of the `composer.json` in this project to the next semantic version of the project (we'll use `1.5.2` for our example) and a PR opened.

Once this PR is approved and merged, on the master branch, a git tag should be added that prepends a `v` to that semver number just assigned.  `git tag v1.5.2`.  This tag should the get pushed to github.

Now to consume this version in Grails, ensure that the version matching spec in the `magento/app/code/local/Grails/composer.json` will accept that version, updating as necessary and opening a PR with Grails.

Grails will consume the new version of this package upon deployment as `composer update` is run as part of the `bin/dep` deploy process.

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
