---
layout: example
title: Add Sunday Values to Monday
description: Example showing how to add Sunday's values to Monday's data in Power BI
pbix_file: /Add Sunday Values to Monday/AddSundayValuesToMondayExample.pbix
---

## Overview

This example demonstrates how to add Sunday's values to Monday's data in Power BI, which is a common requirement in business reporting where weekend data needs to be included in Monday's totals.

## Solution

The solution involves creating a calculated column that identifies the correct date to use for each row, taking into account whether the current date is a Monday (in which case we need to add Sunday's values) or any other day of the week.

## How to Use

1. Download the Power BI file using the link above
2. Open the file in Power BI Desktop
3. Review the calculated columns and measures in the data model
4. Modify the solution to fit your specific data structure

## Key Components

- Date table with day of week information
- Calculated column for date adjustment
- Measures for aggregating the adjusted values

## Related Examples

- Date calculations in Power BI
- Working with week-based reporting
- Handling weekend data in business reports
