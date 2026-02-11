---
#theme: midnight
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

Click on the table headers to sort.

<!-- GENERATE HTML TABLE HERE -->
<table id="zoneTable">
    <tr>
        <th onclick="sortTable(0)">Field</th>
        <th onclick="sortTable(1)">Group</th>
        <th onclick="sortTable(2)">Level</th>
        <th onclick="sortTable(3)">Division</th>
        <th onclick="sortTable(4)">Job Family</th>
        <th onclick="sortTable(5)">Job Type</th>
        <th onclick="sortTable(6)">Affected Employees</th>
        <th onclick="sortTable(7)">Reductions Required</th>
        <th onclick="sortTable(8)">Percentage</th>
    </tr>
</table>

<script>
function sortTable(n) {
  var table, rows, switching, i, x, y, shouldSwitch, dir, switchcount = 0;
  table = document.getElementById("zoneTable");
  switching = true;
  // Set the sorting direction to ascending:
  dir = "asc";
  /* Make a loop that will continue until
  no switching has been done: */
  while (switching) {
    // Start by saying: no switching is done:
    switching = false;
    rows = table.rows;
    /* Loop through all table rows (except the
    first, which contains table headers): */
    for (i = 1; i < (rows.length - 1); i++) {
      // Start by saying there should be no switching:
      shouldSwitch = false;
      /* Get the two elements you want to compare,
      one from current row and one from the next: */
      x = rows[i].getElementsByTagName("TD")[n];
      y = rows[i + 1].getElementsByTagName("TD")[n];
      /* Check if the two rows should switch place,
      based on the direction, asc or desc: */
      if (dir == "asc") {
        if (x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
          // If so, mark as a switch and break the loop:
          shouldSwitch = true;
          break;
        }
      } else if (dir == "desc") {
        if (x.innerHTML.toLowerCase() < y.innerHTML.toLowerCase()) {
          // If so, mark as a switch and break the loop:
          shouldSwitch = true;
          break;
        }
      }
    }
    if (shouldSwitch) {
      /* If a switch has been marked, make the switch
      and mark that a switch has been done: */
      rows[i].parentNode.insertBefore(rows[i + 1], rows[i]);
      switching = true;
      // Each time a switch is done, increase this count by 1:
      switchcount ++;
    } else {
      /* If no switching has been done AND the direction is "asc",
      set the direction to "desc" and run the while loop again. */
      if (switchcount == 0 && dir == "asc") {
        dir = "desc";
        switching = true;
      }
    }
  }
}
</script>