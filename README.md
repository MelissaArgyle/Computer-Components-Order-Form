<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
   
    /*background color and description text color*/
        body {
            font-family: Arial, sans-serif;
            background-image:linear-gradient(#ACD3AB, #D4E8D4);
            color: #2F5649;
        }
       
    /*form width, margin, colour, etc*/
        .form-container {
            width: 70%;
            margin: auto;
            padding: 20px;
            border: 1px dotted #ccc;
            border-color:green;
            border-radius: 4px;
            margin-top: 20px;
            background-color: beige;
        }
       
     /*header appearence*/
        h2 {
        color: #2A637A;
        font-family: Geneva;
        Align: Centre;
        text-shadow: 1px 1px 2px #79BEA8;
        }
         
     /*input-box for component selection details*/
        .form-container input, .form-container select {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            display: inline-block;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
       
     /*Color of 'Submit Order' font changes when icons moves over it*/
        .form-container input:hover, .form-container submitorder:hover {
        color: #559E54;
         box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24),0 17px 50px 0 rgba(0,0,0,0.19);
        }
               
     /*Add Item button details*/
        .form-container button {
            background-color: #448D76;
            color: white;
            border: none;
            cursor: pointer;
            margin: 10px;
            font-size: 16px;        
        }
       
     /*Appearance of "Add Item" button when mouse moves over it*/
        .form-container button:hover {
            background-color: #45a049;
            margin:10px 0;
            box-shadow: 0 10px 11px 0 rgba(0,0,0,0.14),0 11px 30px 0 rgba(0,0,0,0.11);
        }                            
     
           
    /*Appearance of 'Component selection box' when icon moves over it*/
       select:hover {
        background:#448D76;
        color: white;
        }
       
     /*Appearance of all the input boxes*/
        .form-container input {
        background: white;        
        }
               
        .summary {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
               
    </style>
</head>
<body>
    <div class="form-container">
        <h2 align="center">Computer Components Order Form</h2>
        <form id="orderForm">
        <label for="name">Name and Surname:</label>
        <input type="text" id="name" name="name" placeholder="Your name and surname.." required>
        <label for="Cellphone">Contact details:</label>
        <input type="text" id="cellphone" name="cellphone" placeholder="Contact number.." required>


            <label for="email">Email Address:</label>
            <input type="email" id="email" name="email" placeholder="Your email address.." required>

            <label for="component">Component</label>
            <select id="component" name="component">
                <option value="select item">Select item</option>
                <option value="cpu">CPU</option>
                <option value="gpu">GPU</option>
                <option value="ram">RAM</option>
                <option value="ssd">SSD</option>
                <option value="keyboard">Keyboard</option>
                <option value="mouse">Mouse</option>
                <option value="motherboard">Motherboard</option>
                <option value="vga">VGA</option>
                <option value="psu">Power Supply</option>
                <option value="printer">Printer</option>
                <option value="fan">Cooling Fan</option>
                <option value="monitor">Monitor</option>
            </select>

            <div id="additionalOptions"></div>

            <label for="quantity">Quantity</label>
            <input type="number" id="quantity" name="quantity" min="1" placeholder="Quantity.." value="1">

            <button type="button" id="addItem">Add Item</button><br>

            <label for="total">Total Cost</label>
            <input type="text" id="total" name="total" readonly>

            <div id="selectedItems"></div>

            <label for="delivery">Delivery Option</label>
            <div class="delivery-option">
            <label for="collect">Collect</label>
                <input type="radio" id="collect" name="delivery" value="collect" checked>
                <label for="deliver">Deliver</label>
                <input type="radio" id="deliver" name="delivery" value="deliver">
            </div>
            <div class="address-field" id="addressField">
                <label for="address">Delivery Address</label>
                <input type="text" id="line1" name="line1" placeholder="Address line 1..">
                <input type="text" id="line2" name="line2" placeholder="Address line 2..">
                <input type="text" id="town" name="town" placeholder="City/Town..">
                <input type="text" id="postal" name="postal" placeholder="Postal code..">

            <div class="submitorder" id="submitorder">            
            <input type="submit" id="submit" value="Submit Order" style="font-size:16px;background-color:#448D76;color:white;">
              
    </div>

    <script>
    /*Price in Euor for each computer component*/
        var prices = {
            cpu: 200,
            gpu: 400,
            ram: 100,
            ssd: 150,
            keyboard: 50,
            mouse: 20,
            motherboard: 250,
            vga: 120,
            psu: 80,
            printer: 300,
            fan: 30,
            monitor: 180
        };
	/*When selecting a component, choose the type/option*/
        var options = {
            cpu: ['2.3 GHz', '3.0 GHz', '3.8 GHz'],
            gpu: ['NVIDIA', 'AMD', 'Intel'],
            ram: ['8GB', '16GB', '32GB'],
            ssd: ['256GB', '512GB', '1TB'],
            keyboard: ['Mechanical', 'Membrane', 'Wireless'],
            mouse: ['Optical', 'Laser', 'Wireless'],
            motherboard: ['ATX', 'Micro ATX', 'Mini ITX'],
            vga: ['1080p', '1440p', '4K'],
            psu: ['500W', '650W', '750W'],
            printer: ['Laser', 'Inkjet', 'Dot matrix'],
            fan: ['120mm', '140mm', '200mm'],
            monitor: ['24"', '27"', '32"']
        };
		
        var componentSelect = document.getElementById('component');
        var additionalOptionsDiv = document.getElementById('additionalOptions');
        var quantityInput = document.getElementById('quantity');
        var totalInput = document.getElementById('total');
        var selectedItemsDiv = document.getElementById('selectedItems');
        var addressField = document.getElementById('addressField');
       
       /*Creates additional dropdown list when choosing a component*/
       componentSelect.addEventListener('change', function() {
            additionalOptionsDiv.innerHTML = '';
            var selectedComponent = this.value;
            var componentOptions = options[selectedComponent];
       
            
       /*Function to choose component options from dropdown list*/
            if (componentOptions) {
                var optionSelect = document.createElement('select');
                optionSelect.name = 'option';
                componentOptions.forEach(function(option) {
                    var optionElement = document.createElement('option');
                    optionElement.value = option;
                    optionElement.textContent = option;
                    optionSelect.appendChild(optionElement);
                });
                additionalOptionsDiv.appendChild(optionSelect);
            }
        });
        
      /*Function to add item into list when selected by user*/
        function addItem() {
            var selectedComponent = componentSelect.value;
            var selectedOption = additionalOptionsDiv.querySelector('select[name="option"]') ? additionalOptionsDiv.querySelector('select[name="option"]').value : '';
            var quantity = quantityInput.value;
            var price = prices[selectedComponent];
            var total = price * quantity;

            var selectedItemDiv = document.createElement('div');
            selectedItemDiv.textContent = selectedComponent.charAt(0).toUpperCase() + selectedComponent.slice(1) +
                                          (selectedOption ? ' - ' + selectedOption : '') +
                                          ' x ' + quantity + ' = €' + total;
            selectedItemsDiv.appendChild(selectedItemDiv);

            var currentTotal = parseFloat(totalInput.value.replace('€', '') || 0);
            
      /*Function for Total Cost, which adds newly selected item to current total*/
            totalInput.value = '€' + (currentTotal + total);
        }

        document.getElementById('addItem').addEventListener('click', addItem);

        document.getElementById('orderForm').addEventListener('submit', function(event) {
            event.preventDefault();

            var total = parseFloat(document.getElementById('total').value.replace('€', ''));
     /*Error message if no item is selected*/
           if (isNaN(total) || total <= 0) {
                alert('Invalid total cost!');
                return;
            }

            alert('Form submitted successfully!');
        });

        // Initialize total cost
        totalInput.value = '€0';
    </script>

