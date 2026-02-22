<head>
    <style>
        .centered-padded-div {
            width: 50%; /* Set a specific width for the div to be centered within its container (e.g., the page) */
            margin: 0 auto; /* This centers the block-level div horizontally */
            padding: 20px; /* Adds 20px of padding to all sides (top, right, bottom, left) */
            background-color: #f0f0f0; /* Optional: adds a background color for visualization */
            border: 1px solid #ccc; /* Optional: adds a border for visualization */
            text-align: left; /* Ensures the text inside the div is left-aligned */
            box-sizing: border-box; /* Ensures padding is included in the total width calculation, preventing overflow */
        }
    </style>
</head>

# Homework 3

## Part 1 -- Competition Rules

Rule 1: ``Interactions with grey team infrastructure that are not outlined as allowed will result in scoring deductions``

Description:

&emsp;``` 
Users red team may only interact with the scoring box's proxy, while blue team may only interact with the wazuh dashboard web interface. Grey team infrastructure includes critical services and boxes that are used to properly administrate the competition. Interfering with any of the machines in a way that breaks the rules will result in point deductions at the discretion of team Foxtrot.
```