---
theme: midnight
title: Contracts
toc: false
---

# Contracts

```js
// attach data
const contracts = FileAttachment("data/selected_contracts.csv").csv({typed: true});
```

<!-- Create a div where the graph will take place -->


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

<div class="grid grid-cols-1">
  <div class="card">
    ${resize((width) => contractsBarChart(contracts, {width}))}
  </div>
</div>