

###1.ejs
- First example
```js
<% for (var i = 0;i <people.length;i==) { %>
  <% if(people[i].length===2) { %>
    This is time
  <% } %>
  <strong><%= people[i] %></strong></br>
<% } %>
```

- render
```js
var data = {name: "Ke", age:1 };
var result = new EJS({url:'template.ejs'}).render(data);
$("#placeholder").html(result);
```

- put it together
```js
<script type="text/javascript">
  var data = {name: "Ke", age:1};
  var str = $("#testTemplate").html();
  var result = new EJS({text:str}).render(data);
  $("#placeholder").html(result);
</script>

<script id="testTemplate" type="text/x-ejs-template">
  <h1>Hello <%= name %></h1>
  You are <%= age %> years old.
</script>
```

- Get data using jQuery's getJSON method
```js
<body>
  <div id="resdiv"></div>
  <script>
    $(function(){
      $.getJSON("people.json", function(res) {
        var str = $("#testTemplate").html();
        var data = {people:res};
        var rendered = new EJS({text:str}).render(data);
        $("#resdiv").html(rendered);
      });
    });
  </script>
  
  <script id="testTemplate" type="text/x-ejs-template">
    <% for(var i=0;i<people.length; i++) { % >
      <%= people[i].name %>
    <%} %>
  </script>
</body>
```

- helpers
