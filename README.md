# ABOUTME
Ulrich
Ulrich#4976

Ulrich — 05/25/2023 1:17 PM
exercise qoui?
Edwin Tienteu — 05/25/2023 9:35 PM
6 ou 7
Edwin Tienteu — 05/26/2023 1:16 PM
Attachment file type: acrobat
Del.pdf
101.80 KB
Edwin Tienteu — 05/26/2023 1:26 PM
context= new KitchenContext();

            //setting up orders
            context.Orders.Load();
            orderBindingSource.DataSource = context.Orders.Local.ToBindingList();

            //setting up products
            context.Products.Load();
            productBindingSource.DataSource = context.Products.Local.ToBindingList();

            //setting up service
            context.Services.Load();
            serviceBindingSource.DataSource = context.Services.Local.ToBindingList();

            //setting up service
            //context.Customers.Load();
            //customerBindingSource.DataSource = context.Customers.Local.ToBindingList();


            //setting up service
            //context.OrderItems.Load();
            //orderItemBindingSource.DataSource = context.OrderItems.Local.ToBindingList();
            CustomerFullName();
Edwin Tienteu — 05/26/2023 9:53 PM
<connectionStrings>
        <add name="KitchenSupply"
        connectionString="Data Source=(local)\SQLEXPRESS;Initial Catalog=KitchenSupply;Integrated Security=True;TrustServerCertificate=True"
/>
    </connectionStrings>
=> optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["KitchenSupply"].ConnectionString);
Ulrich — 06/01/2023 10:46 AM
COPPY TOUT TON app.Post dans to api tu send empeu
Edwin Tienteu — 06/01/2023 10:51 AM
Attachment file type: acrobat
Api.pdf
113.88 KB
Ulrich — 06/01/2023 10:55 AM
je parle de express pas ca
Edwin Tienteu — 06/01/2023 11:55 AM
app.post("/api/menuitems/:id(\d{3})", async function (request, response) {
  let menuData = request.body;
  let theMenu = new MenuItem(
    menuData.id,
    menuData.category,
    menuData.description,
    menuData.price,
    menuData.vegetarian
  );

  try {
    let ok = await acc.addItem(theMenu);
    if (ok) {
      response.status(201).json({ err: null, data: true });
    } else {
      response
        .status(409)
        .json({ err: item ${menuData.id} already exists, data: null });
    }
    if (menuData.id == 0 || menuData.id === null) {
      response.status(400).json({ err: constructor error, data: null });
    }
  } catch (err) {
    response.status(500).json({ err: "add aborted" + err, data: null });
  }
});
Ulrich — 06/01/2023 11:08 PM
app.post('/api/menuitems/:id(\d{3})', async (req, res) => {
    logger.logRequestInfo(req);
    try {
        let menuitem = req.body;

        // Extract the id from the URL
        let id = parseInt(req.params.id);
        if (isNaN(id)  id < 100  id > 999) {
            res.status(400).json({ err: "MenuItem constructor error: Invalid id", data: null });
            return;
        }

        // Check if all required fields are present and valid
        if (!(menuitem.category && menuitem.description && menuitem.price && typeof menuitem.vegetarian !== 'undefined')) {
            res.status(400).json({ err: "MenuItem constructor error: Missing or invalid fields in request", data: null });  // Bad Request
            return;
        }

        // Add validation for data types
        if (typeof menuitem.category !== 'string'  menuitem.category.length < 3  typeof menuitem.description !== 'string'  isNaN(menuitem.price)  menuitem.price < 0 || typeof menuitem.vegetarian !== 'boolean') {
            res.status(400).json({ err: "MenuItem constructor error: Invalid field data", data: null });  // Bad Request
            return;
        } else {
            let menuitemObj = new MenuItem(
                id,
                menuitem.category,
                menuitem.description,
                menuitem.price,
                menuitem.vegetarian,
            );

            let ok = await MenuItemAccessor.addItem(menuitemObj);
            if (ok) {
                res.status(201).json({ err: null, data: true });
            } else {
                res.status(409).json({ err: MenuItem constructor error: item ${menuitemObj.id} already exists, data: null }); // conflict
            }
        }
    } catch (err) {
        res.status(500).json({ err: MenuItem constructor error: ${err.message}, data: null });
    }
});
Edwin Tienteu — 06/02/2023 12:32 AM
-------------------------------------------------------
app.post("/api/menuitems/:id(d{3})", async function (request, response) {
  let menuData = request.body;
  let theMenu = new MenuItem(
    menuData.id,
    menuData.category,
    menuData.description,
    menuData.price,
    menuData.vegetarian
  );

  try {
    let ok = await acc.addItem(theMenu);
    let id = Number(request.params.id);
    if (isNaN(id)  id < 100  id > 999) {
      res
        .status(400)
        .json({ err: "MenuItem constructor error: Invalid id", data: null });
      return;
    }

    // Check if all required fields are present and valid
    if (
      !(
        menuData.category &&
        menuData.description &&
        menuData.price &&
        typeof menuData.vegetarian !== "undefined"
      )
    ) {
      res
        .status(400)
        .json({
          err: "MenuItem constructor error: Missing or invalid fields in request",
          data: null,
        }); // Bad Request
      return;
    }
    if (ok) {
      response.status(201).json({ err: null, data: true });
    } else {
      response
        .status(409)
        .json({ err: item ${menuData.id} already exists, data: null });
    }
    // if (menuData.id == 0  menuData.id === null  menuData.id === undefined) {
    //   response.status(400).json({ err: constructor error, data: null });
    // }
  } catch (err) {
    response.status(500).json({ err: "add aborted" + err, data: null });
  }
});
app.get("/api/menuitems", async function (request, response) {
  try {
    let data = await acc.getAllItems();
    response.status(200).json({ err: null, data: data });
  } catch (err) {
    response.status(500).json({ err: "could not read data" + err, data: null });
  }
});
Ulrich — 06/02/2023 1:31 AM
Image
const express = require('express');
const bodyParser = require('body-parser');
const { MenuItem } = require("./entity/MenuItem");
const MenuItemAccessor = require('./db/MenuItemAccessor');
const app = express();
app.use(bodyParser.json());
Expand
server.txt
7 KB
Edwin Tienteu — 06/02/2023 10:29 AM
Test un peu le fichier si
Attachment file type: archive
A4_Edwin.zip
5.96 MB
Ulrich — 06/02/2023 10:49 AM
Image
Ulrich — 06/02/2023 11:13 AM
// Just some helpful methods for displaying to console
const path = require("path");

exports.logRequestInfo = logRequestInfo;
exports.logError = logError;

Expand
logg.txt
1 KB
Edwin Tienteu — 06/04/2023 11:23 AM
Image
Image
Edwin Tienteu
 started a call that lasted 17 minutes.
 — 06/04/2023 11:59 AM
Ulrich — 06/04/2023 12:07 PM
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <connectionStrings>
        <add name="emp"
             connectionString="server=localhost;database=empdb;userid=cathy;password=cathy" />
    </connectionStrings>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.8" />
    </startup>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="System.Runtime.CompilerServices.Unsafe" publicKeyToken="b03f5f7f11d50a3a" culture="neutral" />
        <bindingRedirect oldVersion="0.0.0.0-6.0.0.0" newVersion="6.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
        <assemblyIdentity name="System.Memory" publicKeyToken="cc7b13ffcd2ddd51" culture="neutral" />
        <bindingRedirect oldVersion="0.0.0.0-4.0.1.2" newVersion="4.0.1.2" />
      </dependentAssembly>
    </assemblyBinding>
  </runtime>
</configuration>
Ulrich — 06/04/2023 12:33 PM
// View tab: Populate the listbox with all tables found in the database
                string sql2 = "SHOW TABLES FROM empdb";
                MySqlCommand command1 = new MySqlCommand(sql2, connection);
                MySqlDataReader reader = command1.ExecuteReader();

                while (reader.Read())
                {
                    lstTables.Items.Add(reader[0].ToString());

                }
                LogSqlCommand(sql2);
                reader.Close();
string sql1 = "DROP TABLE IF EXISTS families";
                command = new MySqlCommand(sql1, connection);
                command.ExecuteNonQuery();
Edwin Tienteu — 06/04/2023 10:38 PM
Image
ca donn un error
Ulrich — 06/04/2023 10:39 PM
// Set the start date and end date
                string sql3 = "SELECT MIN(hire_date), MAX(hire_date) FROM employees";
                command = new MySqlCommand(sql3, connection);
                reader = command.ExecuteReader();
                LogSqlCommand(sql3);

                if (reader.Read())
                {
                    DateTime minDate = reader.GetDateTime(0);
                    DateTime maxDate = reader.GetDateTime(1);
                    // Add code here to set your start date and end date
                    dtpStartDate.Text = minDate.ToString();
                    dtpStartDate.CustomFormat = "M/d/yyyy";
                    dtpEndDate.Text = maxDate.ToString();
                    dtpEndDate.CustomFormat = "M/dd/yyyy";
                }
                reader.Close();
ce que jai use
Edwin Tienteu — 06/04/2023 10:50 PM
pour delete les invalid dates il me manque quleque chose?
Image
ca ne update pas les invalid date
Ulrich — 06/04/2023 11:34 PM
private void lstTables_SelectedIndexChanged(object sender, EventArgs e)
        {
            // Set the combobox for searching to display nothing as tables get selected
            cboSearch.SelectedIndex = -1;

            // Get selected table name
            string selectedTable;
            if (lstTables.SelectedItem != null)
            {
                selectedTable = lstTables.SelectedItem.ToString();

                // Run a query to show all fields and records for the table selected
                DataTable dt = new DataTable();

                MySqlCommand cmd = new MySqlCommand($"SELECT * FROM {selectedTable}", connection);
                MySqlDataAdapter adapter = new MySqlDataAdapter(cmd);
                adapter.Fill(dt);

                // Set up binding source and link it to datagrid and binding navigator
                BindingSource bindingSource = new BindingSource();
                bindingSource.DataSource = dt;

                // Set the datagrid data source to the binding source
                dgvResults.DataSource = bindingSource;

                // Set the binding navigator binding source to the binding source created
                bdnDataGrid.BindingSource = bindingSource;

                // Display the binding navigator
                bdnDataGrid.Visible = true;
            }

        }
Edwin Tienteu — 06/05/2023 9:47 AM
Image
Ulrich — 06/09/2023 10:20 AM
onst { id, category, description, price, vegetarian } = panelItem  {};

      return (
          <div id="inputPanel" className="hidden">
              <div className="input-group mb-3">
                  <span className="input-group-text">ID</span>
                  <input id="idInput" type="number" min="100" max="999" onChange={(e) => inputChangeHandler('id', e.target.value)} value={id  ''} className="form-control"  disabled={!isInsert}  />
              </div>
Ulrich — 10/17/2023 9:24 AM
Java A3 Lab
Attachment file type: archive
A3_MadonfaUlrich.zip
850.14 KB
Edwin Tienteu — 10/29/2023 1:48 PM
private void reportSummaryByDestination() {
        // TODO-C2
        txtOutput.setText("Summary Report By Destination\n\n");
                int addNum = 0;
        int addWeightNum = 0;
        int bahNum = 0;
        int bahWeightNum = 0;
        int bkkNum = 0;
        int bkkWeightNum = 0;
                   for (Flight X : allFlights) {
                if (X.getDestination().getLocationCode().equals("ADD")) {
                    addNum++;
                    addWeightNum += X.calculateWeight();
                }if (X.getDestination().getLocationCode().equals("BAH")) {
                    bahNum++;
                    bahWeightNum += X.calculateWeight();
                }if (X.getDestination().getLocationCode().equals("BKK")) {
                    bkkNum++;
                    bkkWeightNum += X.calculateWeight();
                }
            }
     txtOutput.append("         " + addNum + " Flights to ADD"  +" \t\t Weight = " + String.format("%,d", addWeightNum) + " \n ");
            txtOutput.append("         " + bahNum + " Flights to BAH" +" \t\t Weight = " + String.format("%,d", bahWeightNum) + " \n ");
            txtOutput.append("         " + bkkNum + " Flights to BKK"  +" \t\t Weight = " + String.format("%,d", bkkWeightNum) + " \n ");


}
Image
Je suis entrain de completer
Je vais finir avant de go au work
Edwin Tienteu — 11/11/2023 9:28 AM
<!DOCTYPE html>

<html>

<head>
    <meta charset="UTF-8">
    <title>Validation Demo - PHP</title>
    <style>
        .inputControl {
            clear: left;
        }

        .label {
            width: 150px;
            float: left;
        }

        button {
            margin-left: 150px;
            margin-top: 20px;
            font-size: 1.25em;
            width: 150px;
        }

        .inputError {
            color: red;
            font-family: monospace;
        }
    </style>
    <script>
    </script>
</head>

<body>

    <h1>Validation Demo - PHP</h1>

    <form method="POST" action="validation.php">

        <div class="inputControl">
            <div class="label">Number of choice:</div>
            <input id="choiceNum" name="choiceNum" type="number" min="1" max="100" value="1">

        </div>


        <button type="submit">Done</button>
    </form>

</body>

</html> 
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Validation Demo - PHP - Results</title>
</head>

<body>
    <?php
    session_start();
    $questionID = $_SESSION['QuestionID'];
    $title = $_SESSION['title'];
    $points = $_SESSION['Points'];
    $answer = $_SESSION['answer'];
    $allChoices = $_SESSION['choices'];
    // $optionNum = $_POST["choiceNum"];
    //$message = "User #" . $questionID . " registered with email '" . $title . "' and password '" . $points . "'.";
    // for ($i = 0; $i < $optionNum; $i++) {
    //     $allChoices[$i] = $_POST["choice" . $i];
    // }
    ?>

    <h1>Validation Demo - JS - Results</h1>
    <p>
        <?php //echo $message; ?>
        <?php
        $obj = (object) [
            'QuestionID' => $questionID,
            'Title' => $title,
            'Choices' => $allChoices,
            'Answer' => $answer,
            'points' => $points
        ];

        echo json_encode($obj);
        ?>
    </p>
</body>

</html>
Edwin Tienteu — 11/11/2023 10:35 AM
<?php
    session_start();
    $questionID = "";
    $title = "";
    $points = 0;
    $answer = "";
Expand
message.txt
7 KB
Ulrich — 11/11/2023 10:45 AM
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Validation Assignment - HTML&PHP</title>
    <link rel="stylesheet" type="text/css" href="style.css">
Expand
message.txt
5 KB
Edwin Tienteu — 11/17/2023 8:38 AM
Image
Ulrich — 11/17/2023 8:56 AM
Image
Image
Ulrich — 11/17/2023 9:12 AM
Image
Edwin Tienteu — 11/19/2023 10:08 AM
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/JSP_Servlet/Servlet.java to edit this template
 */
package entity;

Expand
message.txt
5 KB
LoadCatalog
Je go au church
Ulrich — 11/19/2023 10:35 AM
Yes bro merci
Edwin Tienteu — Today at 11:12 AM
<h1>Hi, I'm Josh! <br/><a href="https://github.com/joshmadakor1">Programmer</a>, <a href="https://www.linkedin.com/in/joshmadakor/">Cybersecurity Professional</a>, <a href="https://www.youtube.com/c/joshmadakor">YouTuber</a></h1>

<h2>👨‍💻 Software Development Projects:</h2>

- <b>Data Structures and Algorithms Practice (AlgoExpert)</b>
  - [Praciting DS & Algos in Python](https://github.com/joshmadakor1/Algorithms-Practice)
Expand
message.txt
4 KB
<h1>Hi, I'm Josh! <br/><a href="https://github.com/joshmadakor1">Programmer</a>, <a href="https://www.linkedin.com/in/joshmadakor/">Cybersecurity Professional</a>, <a href="https://www.youtube.com/c/joshmadakor">YouTuber</a></h1>

<h2>👨‍💻 Software Development Projects:</h2>

- <b>Data Structures and Algorithms Practice (AlgoExpert)</b>
  - [Praciting DS & Algos in Python](https://github.com/joshmadakor1/Algorithms-Practice)
Expand
message.txt
4 KB
﻿
Edwin Tienteu
edwintienteu
<h1>Hi, I'm Josh! <br/><a href="https://github.com/joshmadakor1">Programmer</a>, <a href="https://www.linkedin.com/in/joshmadakor/">Cybersecurity Professional</a>, <a href="https://www.youtube.com/c/joshmadakor">YouTuber</a></h1>

<h2>👨‍💻 Software Development Projects:</h2>

- <b>Data Structures and Algorithms Practice (AlgoExpert)</b>
  - [Praciting DS & Algos in Python](https://github.com/joshmadakor1/Algorithms-Practice)
- <b>Full Stack Web App (React, NodeJS, Azure, and Machine Learning Components)</b>
  - [Image Analysis Middleware](https://github.com/joshmadakor1/4chan-Image-Analysis-Middleware-C964) <b><i>(Potentially NSFW)</b></i>
- <b>PowerShell</b>
  - [Windows EventLog: Failed RDP Logins Source IP to full GeoData Conversion](https://github.com/joshmadakor1/Sentinel-Lab)
  - [JWipe (Disk Wiping Utility)](https://github.com/joshmadakor1/Jwipe.PowerShell)
  - [Active Directory Bulk User Creation](https://github.com/joshmadakor1/AD_PS)
  - [FIM (File Integrity Monitor)](https://github.com/joshmadakor1/PowerShell-Integrity-FIM)
- <b>C# (.NET Desktop Applications)</b>
  - [Ransomware Proof of Concept (Encrypter)](https://github.com/joshmadakor1/EncrypterPOC)
  - [Ransomware Proof of Concept (Decrypter)](https://github.com/joshmadakor1/DecrypterPOC)
  - [Keylogger with Email Capability](https://github.com/joshmadakor1/Key-Logger-With-Email)
- <b>Python</b>
  - [Package Delivery Application (Datastructures and Algorithms Demo)](https://github.com/joshmadakor1/Package-Delivery-Pathfinding-Algorithm)

<h2>📺 Popular YouTube Videos</h2>

- [How to get into Cybersecurity Starting From Zero](https://www.youtube.com/watch?v=a83ASGn_V_s)
- [A Day in the Life of a Cybersecurity Anayst](https://www.youtube.com/watch?v=uHy3oM7NnoU)
- [How to Create a KeyLogger (C#)](https://www.youtube.com/watch?v=N-L9hklSlNk)
- [Ransomware Demonstration (C#)](https://www.youtube.com/watch?v=OfvdQeh79s0)
- [Is WGU Legit?](https://www.youtube.com/watch?v=E2MwRWxDBkA)

<h2> 🤳 Connect with me:</h2>

[<img align="left" alt="JoshMadakor | YouTube" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/youtube.svg" />][youtube]
[<img align="left" alt="JoshMadakor | Twitter" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/twitter.svg" />][twitter]
[<img align="left" alt="JoshMadakor | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]
[<img align="left" alt="JoshMadakor | Instagram" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/instagram.svg" />][instagram]

[twitter]: https://twitter.com/joshmadakor
[youtube]: https://www.youtube.com/c/joshmadakor
[instagram]: https://www.instagram.com/joshmadakor/
[linkedin]: https://linkedin.com/in/joshmadakor

<!--
**joshmadakor1/joshmadakor1** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
message.txt
4 KB
