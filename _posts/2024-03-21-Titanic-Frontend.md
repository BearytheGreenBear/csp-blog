---
toc: true
title: Titanic Frontend
courses: { compsci: {week: 27} }
type: hacks
---

<div class="container">
  <form id="passengerForm" action="javascript:calculateSurvival()">
      <p><label>
          Name:
          <input class="userInput" type="text" name="name" id="name" required>
      </label></p>
      <p><label>
          Your Class:
          <input class="userInput" type="checkbox" name="pclass" id="pclass" value="1"> 1st Class
          <input class="userInput" type="checkbox" name="pclass" id="pclass" value="2"> 2nd Class
          <input class="userInput" type="checkbox" name="pclass" id="pclass" value="3"> 3rd Class
      </label></p>
      <p><label>
          Gender:
          <input class="userInput" type="checkbox" name="sex" id="sex" value="male"> Male
          <input class="userInput" type="checkbox" name="sex" id="sex" value="female"> Female
      </label></p>
      <p><label>
          Age:
          <input class="userInput" type="text" name="age" id="age" required>
      </label></p>
      <p><label>
          Number of Siblings and Spouse Aboard:
          <input class="userInput" type="text" name="sibsp" id="sibsp" required>
      </label></p>
      <p><label>
          Number of Parents/Children Aboard:
          <input class="userInput" type="text" name="parch" id="parch" required>
      </label></p>
      <p><label>
          Fare:
          <input class="userInput" type="text" name="fare" id="fare" required>
      </label></p>
      <p><label>
          Embarked:
          <input class="userInput" type="checkbox" name="embarked" id="embarked" value="C"> Cherbourg
          <input class="userInput" type="checkbox" name="embarked" id="embarked" value="Q"> Queenstown
          <input class="userInput" type="checkbox" name="embarked" id="embarked" value="S"> Southampton
      </label></p>
      <p><label>
          Alone:
          <select class="userInput" name="alone" id="alone" required>
              <option value="true">Yes</option>
              <option value="false">No</option>
          </select>
      </label></p>
      <p>
          <button id="sendButton" type="submit">Send</button>
      </p>
  </form>
</div>
9:22
<h1 id="h1">Will you survive?</h1>

<div class="container">
  <form id="passengerForm" action="javascript:calculateSurvival()">
      <p><label>
          Name:
          <input class="userInput" type="text" name="name" id="name" required>
      </label></p>
      <p><label>
          Your Class:
          <input class="userInput" type="checkbox" name="pclass" id="pclass" value="1"> 1st Class
          <input class="userInput" type="checkbox" name="pclass" id="pclass" value="2"> 2nd Class
          <input class="userInput" type="checkbox" name="pclass" id="pclass" value="3"> 3rd Class
      </label></p>
      <p><label>
          Gender:
          <input class="userInput" type="checkbox" name="sex" id="sex" value="male"> Male
          <input class="userInput" type="checkbox" name="sex" id="sex" value="female"> Female
      </label></p>
      <p><label>
          Age:
          <input class="userInput" type="text" name="age" id="age" required>
      </label></p>
      <p><label>
          Number of Siblings and Spouse Aboard:
          <input class="userInput" type="text" name="sibsp" id="sibsp" required>
      </label></p>
      <p><label>
          Number of Parents/Children Aboard:
          <input class="userInput" type="text" name="parch" id="parch" required>
      </label></p>
      <p><label>
          Fare:
          <input class="userInput" type="text" name="fare" id="fare" required>
      </label></p>
      <p><label>
          Embarked:
          <input class="userInput" type="checkbox" name="embarked" id="embarked" value="C"> Cherbourg
          <input class="userInput" type="checkbox" name="embarked" id="embarked" value="Q"> Queenstown
          <input class="userInput" type="checkbox" name="embarked" id="embarked" value="S"> Southampton
      </label></p>
      <p><label>
          Alone:
          <select class="userInput" name="alone" id="alone" required>
              <option value="true">Yes</option>
              <option value="false">No</option>
          </select>
      </label></p>
      <p>
          <button id="sendButton" type="submit">Send</button>
      </p>
  </form>
</div>


<script>
    const url = "http://127.0.0.1:8028/api/titanic/";
    const options = {
        method: 'GET', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
        credentials: 'include', // include, same-origin, omit
        headers: {
            'Content-Type': 'application/json',
},
    };
    
    // Function to get the value of checked checkboxes
    function getCheckedCheckboxValue(name) {
        const checkboxes = document.querySelectorAll(`input[name="${name}"]:checked`);
        if (checkboxes.length > 0) {
            // If at least one checkbox is checked, return its value
            return checkboxes[0].value;
        }
        // If no checkbox is checked, return null
        return null;
    }

    function calculateSurvival() {
        console.log(parseInt(document.getElementById('pclass').value))
        const body = {
            name: document.getElementById('name').value,
            pclass: parseInt(getCheckedCheckboxValue('pclass')),
            sex: getCheckedCheckboxValue('sex'),
            age: document.getElementById('age').value,
            sibsp: document.getElementById('sibsp').value,
            parch: document.getElementById('parch').value,
            fare: document.getElementById('fare').value,
            embarked: getCheckedCheckboxValue('embarked'),
            alone: document.getElementById('alone').value
        };
        const post_options = {
            // ...options, // This will copy all properties from options
            method: 'POST', // Override the method property
            cache: 'no-cache', // Set the cache property
            body: JSON.stringify(body),
            headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': 'include'
            },
        };
        fetch(url, post_options)
            .then(response => {
                // handle error response from Web API
                if (!response.ok) {
                    const errorMsg = response.status;
                    console.log(errorMsg);
                    return;
                }

                // Extract data from response
                return response.json();
            })
            .then(data => {
                // Display information in the h1 tag
                const dataString = data;
                console.log(dataString);
                console.log(dataString[0])
                // Display information in the h1 tag
                const h1 = document.getElementById('h1');
                // Display the stringified data in the h1 tag
                h1.textContent = dataString[0];
                console.log(data);
            })
            // catch fetch errors (ie ACCESS to server blocked)
            .catch(err => {
                console.error(err);
            });
    }
</script>