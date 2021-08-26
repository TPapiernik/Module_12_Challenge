# Module 12 Challenge - Belly Button Plotting - JavaScript [D3.js and Plotly]

## Overview

The purpose of this project is to update and republish a Dynamic Dashboard Webpage
displaying Study Metadata for a collection of volunteers who submitted Belly Button
Bacterial Samples for Analysis to **Improvable Beef**.

Upon completion, the updated dashboard was deployed to the World Wide Web using GitHub Pages.

### Deliverables

1. Create a Horizontal Bar Chart
2. Create a Bubble Chart
3. Create a Gauge Chart
4. Customize the Dashboard

Note: Live version of Webpage Dashboard is hosted here [https://tpapiernik.github.io/Module_12_Challenge/](https://tpapiernik.github.io/Module_12_Challenge/)

*(Must have JavaScript Enabled)*

### Resources

- Software:
	- Bootstrap 3 CSS Components loaded from Cloud via HTML
	- D3.js JavaScript Library loaded from Cloud via HTML
	- Plotly JavaScript Library loaded from Cloud via HTML
- Data:
	- `samples.json`: Data Collection of Samples stored in JavaScript Object Notation Format. Collection of Data from 153 Volunteers. Three primary lists of dictionaries, `names`, `metadata`, and `samples`. Additional information about the structure and contents of `samples.json` is summarized below in Table 1.

**Table 1: samples.json Data Summary**
| Primary List Name	| Field Name      | Brief Description of Contents |
|-----------------------|-----------------|-------------------------------|
|`names`	        | N/A             | Zero-Indexed Dictionary Listing of Volunteer ID Numbers [3-4 digit Double-Quoted 'Integers' stored as strings, in ascending but not necessarily consecutive order]
|                       |                 |
|`metadata`             | `id`            | Corresponds to Key-Value Pair in `names`, in same order
| 			| `ethnicity`     | Double-Quoted Mixed-Case Free Text Strings, Slash Separated for Sub-Ethnicities, some values have additional clarification embedded in Parentheses. 13 Unique Classifications, 4 'null' values present in collection.
|			| `gender`        | Double-Quoted Mixed-Case Single-Character Strings, possible values of 'F', 'M', 'f', 'm', 1 'null' value present in collection.
|			| `age`           | Positive Integer Value representing Age in Years since Birth, ranging between 1 and 68. 3 'null' values present in collection.
|			| `location`      | Double-Quoted Mixed-Case Free Text Strings, describing residence location of Sample Volunteer. Loosely enforced standards on input. Explained in further detail below in **Data Quality**. 27 'null' values present in collection.
|			| `bbtype`        | Double-Quoted Mixed-Case Strings, possible values of 'I', 'i', 'O', 'o', 'Both', or 'Unknown'. 11 'null' values present in collection.
|			| `wfreq`         | Positive Integer Value representing number of simple washes of Sample Volunteer Belly Button per Week (Self-Reported), ranging between 0 and 9. 15 'null' values present in collection, 1 value of '3.5'
|			|                 |
| `samples`		| `id`            | Corresponds to Key-Value Pair in `names`, in same order
|			| `otu_ids`       | Array of OTUs, or Operational Taxonomic Units, of the bacterial species found in each Sample Volunteer's Navel. Arranged in Descending Order corresponding to `sample_values`. 3-4 digit Positive Integer ID Values. 3 Blank IDs present in collection.
|			| `sample_values` | Amount of each OTU measured in Navel of Sample Volunteer. Arranged in Descending Order. Unit of Measurement Unknown.
|			| `otu_labels`    | Double-Quoted Free Text Mixed-Case Strings, listing out in full the Taxonomic Names of the Bacterial Species Identified in Navel of Sample Volunteer. Individual Bacterial Species Semi-Colon Separated. 329 Individual Species Classified in collection, including generic 'Bacteria', 3 Blank values present in collection.


#### Data Quality
The majority of the data present in `samples.json` is consistent, reliable, and complete, especially the data that is currently being used directly in this version of the Dashboard. However, if more complete or in-depth analysis were to be performed in the future, the data collection would have to be cleaned up and regularized a bit. Some of this information is summarized above in **Table 1**, however it will be explained in greater detail here. Most of the improvements and updates could be made in the `metadata` collection. Before moving on to explain that in detail, I will just briefly mention that in the `samples` collection, there are 3 `otu_ids` and 3 `otu_labels` that are specified as Blank. The records for these samples should be double-checked to make sure they were not simply overlooked during data-entry.

In the `metadata` collection, the first area for improvement is filling in 'null' values, of which there are 1 in the `gender` field, 3 in the `age` field, 27 in the `location` field, 11 in the `bbtype` field, and 15 in the `wfreq` field. Unfortunately, without consulting the original records or re-surveying the Study Volunteers, there is no other way at this time to populate these values.

Moving on to the Fields themselves, there are areas for improvement in consistency.

In `ethnicity`, there are 130 entries for 'Caucasian', and 1 for 'White', 1 for 'European', 1 for 'Caucasion' [sic], and 1 each for 'Caucasian/Midleastern' [sic], 'Caucasian/Jewish', and 'Caucasian/Asian'. Asian Respondents are listed as 1 for 'Asian(South)', and 1 for 'Asian(American)', and 1 for 'PacificIslander'. For the sake of consistency, the spelling errors and related categories should be collapsed into a smaller, more-consistent set. Upstream on data-entry, standards for input should be more rigorously enforced to avoid category spread/expansion. If sub-categories are to be allowed, a specific format should be agreed upon to use either Slashes or Parentheses, but not both.

In `gender`, the problems are similar, but on a smaller scale. The two available options of Female or Male are listed under single-character identifiers, but capitalization of inputs has not been enforced. This field could be cleaned up easily be simply converting all non-null data entries to UPPER CASE single-character identifiers.

In `age`, the only problem identified is the presence of 3 'null' values.

In `location`, there are more areas for improvement. First is the 27 'null' values. Second is, many entries only list the Two-Character US State Postal Code for the location, while others only list the City name with no State given. Multi-word Cities are listed in PascalCase with spaces omitted, and there is at least one foreign country entry for 'UK/London'. If the study spans the entire United States and Internationally, the format should be formalized to something more comprehensive such as '\<City\>/\<State|District|Province\>/\<Country\>', and the spaces in City Names should be retained to more easily facilitate comparison against other industry-standard Dictionaries and Atlases. Some of the data entries already in this field are close to this format with the data listed as '\<City\>/\<State\>'.

`bbtype` has 11 'null' values to be filled in, and the same capitalized vs. non-capitalized identifier problem as the `gender` field. This problem at least, could be easily fixed by converting all non-null data entries to UPPER CASE identifiers. However, in addition to this there are 2 entries listed under 'Both', and 1 'Unknown'. Whether these options are medically possible or not, they should probably be dis-allowed to allow for more consistent analysis.

`wfreq` has 15 'null' values to be filled in, and one value of '3.5'. Since a 'wash' should logically be defined as a discreet quantity, I am not sure what would qualify as a 'half-wash'. Zero or Positive Integer values only should be enforced.

## Deliverables

### Deliverable 1

Create a Horizontal Bar Chart: See `index.html`, and `static/js/charts.js`

### Deliverable 2

Create a Bubble Chart: See `index.html`, and `static/js/charts.js`

### Deliverable 3

Create a Gauge Chart: See `index.html`, and `static/js/charts.js`

### Deliverable 4

Customize the Dashboard: See `index.html`, `static/css/style.css`, and `static/images/girl-with-red-hat-Lm1kouW8kAA-unsplash_v1.png`

Customizations Applied:
1. A Background Image was applied to the Jumbotron (Stock Image Attribution linked below Jumbotron, and recorded in `static/images/sources.txt`. Original, un-modified source image retained as `static/images/girl-with-red-hat-Lm1kouW8kAA-unsplash.jpg`)
2. The default background color of the Jumbotron was modified to match the Background Image
3. The default Font was updated for the Jumbotron Header (Google 'Trirong')
4. A descriptive introductory paragraph was added to the top of the dashboard
5. Short Figure Descriptions were added below each dynamic chart in the dashboard

-- END --
