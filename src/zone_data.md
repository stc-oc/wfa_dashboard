---
theme: dashboard
title: Which Zone am I in?
toc: false
---

# Which zone am I in?

Select your information from the menu below to visualize the affected employees and the targeted position cuts (before VDP) in your Zone.

```js

// attach data
const zones = FileAttachment("data/Stats_WFA_tracker - Zones.csv").csv({typed: true});
```

```js
// get selectors
const fields_set = new Set(zones.map(d => d.Field));
const fields = [...fields_set];

const field = view(Inputs.select(fields, {label: "Select field:", sort: true}));
```

```js
const groups_set = new Set(zones.filter(d => d.Field == field).map(d => d.Group));
const groups = [...groups_set];

const group = view(Inputs.select(groups, {label: "Select group:", sort: true}));
```

```js
const levels_set = new Set(zones.filter(d => d.Field == field & d.Group == group).map(d => d.Level));
const levels = [...levels_set];

const level = view(Inputs.select(levels, {label: "Select level:", sort: true}));
```

```js
const divisions_set = new Set(zones.filter(d => d.Field == field & d.Group == group & d.Level == level).map(d => d.Division));
const divisions = [...divisions_set];

const division = view(Inputs.select(divisions, {label: "Select division:", sort: true}));
```

```js
const family_set = new Set(zones.filter(d => d.Field == field & d.Group == group & d.Level == level & d.Division == division).map(d => ''+d["Job Family"]));
const families = [...family_set];

const family = view(Inputs.select(families, {label: "Select job family:", sort: true}));
```

```js
const selected = zones.filter(d => d.Field == field & 
    d.Group == group &
    d.Level == level &
    d.Division == division &
    d["Job Family"] == family);

const affected = selected.length > 0 ? selected[0]["Number of employees"] : "undefined";
const required = selected.length > 0 ? selected[0]["Reductions required"] : "undefined";
const percent = selected.length > 0 ? selected[0]["percent"] : "undefined";
const note = selected.length > 0 ? selected[0]["note"] : "undefined";

console.log(selected);
```

<div class="card">
    <h2>Field <b>${field}</b> Group <b>${group}</b> Level <b>${level}</b> Division <b>${division}</b> Job Family <b>${family}</b></h2>
    <span class="big">Affected: ${affected}</span>
    <br/>
    <span class="big">Positions cut: ${required}</span>
    <br/>
    <span class="big">Percentage: ${percent}</span>
    <br/>
    <span>Note: ${note}</span>
</div>

```js
let table = document.getElementById("zoneTable");

for (let row of zones) {
    let tr = table.insertRow();
    let td = tr.insertCell();
      td.innerHTML = row.Field;
      td = tr.insertCell();
      td.innerHTML = row.Group;  
      td = tr.insertCell();
      td.innerHTML = row.Level;  
      td = tr.insertCell();
      td.innerHTML = row.Division;  
      td = tr.insertCell();
      td.innerHTML = row["Job Family"];  
      td = tr.insertCell();
      td.innerHTML = row["Job Type"];  
      td = tr.insertCell();
      td.innerHTML = row["Number of employees"];  
      td = tr.insertCell();
      td.innerHTML = row["Reductions required"];  
      td = tr.insertCell();
      td.innerHTML = row.percent;  
}
```
---

# Overall Table

<!-- GENERATE HTML TABLE HERE -->
<table id="zoneTable">
    <tr>
        <th>Field</th>
        <th>Group</th>
        <th>Level</th>
        <th>Division</th>
        <th>Job Family</th>
        <th>Job Type</th>
        <th>Affected Employees</th>
        <th>Reductions Required</th>
        <th>Percentage</th>
    </tr>
</table>
