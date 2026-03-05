---
theme: midnight
title: Contracts
toc: false
---

# External Contracts 2020-2025

The Government of Canada is required by law to publish all government contracts worth $10,000 or more. As a Canadian citizen, you have the right to search the data, for example [all contracts signed by Statistics Canada](https://search.open.canada.ca/contracts/?owner_org=statcan).

The dataset shows that, between January 2020 and December 2025, Statistics Canada spent $466,929,619.25 on external contracts. One third of this amount has been disbursed to a single vendor, MICROSOFT CANADA INC., equal to $156,888,221.34 . This makes Microsoft the largest supplier by far, as the second supplier, ORACLE CANADA ULC, received "only" $29,222,997.90 or 6% of the total. 

_Note: Microsoft data may be **double counted**, more on this later._

```js
// attach data
const contracts = FileAttachment("data/selected_contracts.csv").csv({typed: true});
```

In the following plot, we rank Statistics Canada's suppliers. For clarity, we only show the top vendors that absorbed 80% of the total amount spent, i.e. $373,543,695.40 . This plot highlights Statistics Canada's overwhelming dependence on Microsoft products.

```js
function contractsBarChart(data, {width} = {}) {
  return Plot.plot({
    marginLeft: 260,
  x: {axis:"both", grid:true, label: "Total Value of Contracts [CAD]"},
  y: {label: "Vendor Name"},
  marks: [
    Plot.axisX({axis:"top", label: "Total Value of Contracts [CAD]"}),
    Plot.ruleX([0]),
    Plot.barX(data, {y: "vendor_name", x: "sum_total_value", sort: {y: "x", reverse: true}}),
  ]
});
}

```
<!-- Create a div where the graph will take place -->
<div class="grid grid-cols-1">
  <div class="card">
    ${resize((width) => contractsBarChart(contracts, {width}))}
  </div>
</div>

## Microsoft Contracts

Let's focus now on contracts awarded to Microsoft. The two largest contracts are contracts [11349](https://search.open.canada.ca/contracts/record/statcan,C-2022-2023-Q4-00061) and [13769](https://search.open.canada.ca/contracts/record/statcan,C-2024-2025-Q4-00015), which amount to, respectively $42,374,887.00 and $73,756,185.93.

The first contract, [11349](https://search.open.canada.ca/contracts/record/statcan,C-2022-2023-Q4-00061), was first signed on January 7th, 2020 for an initial amount of $452,000 but was amended every year to reach the final amount of $42,374,887 on its termination date, March 31st, 2023.

The second contract, [13769](https://search.open.canada.ca/contracts/record/statcan,C-2024-2025-Q4-00015), was signed on November 1st, 2023 for an inital amount of $41,634,882.77 and then amended to the final amount of $73,756,185.93. This contract is active until March 31st, 2026.

What else do we know about the contracts? They were both signed by Shared Services Canada on behalf of Statistics Canada, and the first contract was about _Computer Services (includes IT solutions / deliverables as well as IT
) Comment: Data center services_ while the second was about _Rental of computer equipment related to production and operations (P&O) environment - All servers, storage, printers, etc. (includes all related parts and peripherals)_. It is safe to assume that these contracts cover **Azure Cloud Services**.

Most notably, the second contract begins roughly at the same time the first contract ends, and the initial amount of the second contract is roughly (but not exactly!) the same as the final amount of the second contract, so it's plausible to assume that the second contract is an extension or amendment of the first one. If this is the case, then the total amount spent is not the sum of the two contracts but the largest of the two numbers, e.g. $73,756,185.93 instead of $116,131,072.93&mdash;so we may need to revise the total amount of money spent. Only reading the actual contracts can give us a clue to what happened.

One last question remains: if the second contract is just an extension of the first one, why is it not a simple amendment? The answer is interesting: while the first contract was competitive, <u>the second contract was non-competitive</u>. This is a story for another time...  
