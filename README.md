

###1.ejs
- First example
```js
<% for (var i = 0;i <people.length;i++) { %>
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
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"http://yidi.me"};
    var str = $("#testTemplate").html();
    var result = new EJS({text:str}).render(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-ejs-template">
  <h1>Hello <%= name %></h1>
  <%= link_to('Homepage', homepage) %><br/>
  <%= img_tag(img, "Picture") %>
</script>
</body>
```

- condition
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"http://yidi.me"};
    var str = $("#testTemplate").html();
    var result = new EJS({text:str}).render(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-ejs-template">
  <h1>Hello <%= name %></h1>
  <%= link_to('Homepage', homepage) %><br/>
  <% if(is_current_page('http://yidi.me')) { %>
    condition1
  <% } else { %>
    condition2
  <% } %>
</script>
</body>
```
- self-defined function

```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  function short(s) {
    return s.subStr(0,10);
  }
  
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"http://yidi.me"};
    var str = $("#testTemplate").html();
    var result = new EJS({text:str}).render(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-ejs-template">
  <h1>Hello <%= name %></h1>
  <%= link_to('Homepage', homepage) %><br/>
  <%= short(bio) %>
</script>
</body>
```
### 2.Handlebars

- basic conditions
```js
{{#each people}}
  {{#if people.firstName}}
    condition1
  {{/if}}
{{/each}}
```
- put it together
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"<i>secret</>"};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  <h1>Hello {{name}}</h1>
  You are {{age}} years old.
  {{{homepage}}}     //To parse html tag. use triple braces
</script>
</body>
```

- Get data using jQuery's getJSON method
```js
<body>
  <div id="resdiv"></div>
  <script>
    var str = $("#testTemplate").html();
    var template = Handlebars.complie(str);
    $(function(){
      $.getJSON("people.json", function(res) {
        var data = {people:res};
        var rendered = template(data);
        $("#resdiv").html(rendered);
      });
    });
  </script>
  
  <script id="testTemplate" type="text/x-ejs-template">
    {{#each people}}
      {{name}}
    {{/each}}
  </script>
</body>
```

- single dimension JSON array
```js
<body>
  <div id="resdiv"></div>
  <script>
    var str = $("#testTemplate").html();
    var template = Handlebars.complie(str);
    $(function(){
      $.getJSON("people.json", function(res) {
        var data = {people:res};
        var rendered = template(data);
        $("#resdiv").html(rendered);
      });
    });
  </script>
  
  <script id="testTemplate" type="text/x-ejs-template">
    {{#each people}}
      {{this}}
      {{!-- or --}}
      {{.}}
    {{/each}}
  </script>
</body>
```

- get outer scope variable
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"<i>secret</>",hobbies:['a','b','c']};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  {{name}}
    {{#each hobbies}}
      {{../name}} likes {{.}} {{!-- use outer scope's name--}}
    {{/each}}
</script>
</body>
```
- boolean
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"<i>secret</>",hobbies:['a','b','c'], isBoolean:true};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  {{name}}
  {{#if isboolean}}
    condition3
  {{else}}
    condition4
  {{/if}}
  
  {{#unless isboolean}}
    condition5
  {{/unless}}
    {{#each hobbies}}
      {{../name}} likes {{.}} {{!-- use outer scope's name--}}
    {{/each}}
</script>
</body>
```

- with keyword
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"<i>secret</>",stats:{key1:"value1",key2:"value2"};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  {{#with stats}}
    <table>
      <tr>
        <td>key1</td>
        <td>{{key1}}</td>
      </tr>
      <tr>
        <td>key2</td>
        <td>{{key2}}</td>
      </tr>
    </table>
  {{/with}}
</script>
</body>
```
- index, first, last
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke", img:"Ke.jpg", homepage:"<i>secret</>",hobbies:['a','b','c'], isBoolean:true};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
    {{#each hobbies}}
      {{if @first}}
        condition
      {{/if}}
      Index:{{@index}}
      Value:{{.}}
      {{if @last}}
        condition
      {{/if}}
    {{/each}}
</script>
</body>
```

- helpers
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  Handlebaars.registerHelper("ageHelper", function(text) {
    if(!text) return "unknown"; //if age field does not exist
    return text;
  });
  
  $(function(){
    var data = {name: "Ke", age:1};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {{ ageHelper age}} years old.
</script>
</body>
```

- little complex helper
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  Handlebaars.registerHelper("ageHelper", function(text) {
    if(!text) return "unknown";
    var age = parseInt(text,10);
    if(age <= 0 || age > 150) return "unknown";
    return text;
  });
  
  $(function(){
    var data = {name: "Ke", age:1};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {{ ageHelper age}} years old.
</script>
</body>
```

- image helper
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  Handlebaars.registerHelper("imageHelper", function(url) {
    return Handlebars.SafeString("<img src=\"" = url = "\">");   //parse HTML code
  });
  
  $(function(){
    var data = {name: "Ke", img:"http://a.jpg"};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  {{imageHelper img}}
</script>
</body>
```

- multi param helper
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  Handlebaars.registerHelper("gravatarurl", function(email, size, def, rating) {
    var str = 'http://...' + md5(email) + 'jpg?';
    if(size) str += '&s='+size;
    if(def) str += '&d' +encodeURI(def);
    if(rating) str += '&r' +rating;
    return str;
  });
  
  $(function(){
    var data = {name: "Ke", email:xxx@xxxx.com,};
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {{ gravatarurl email 100 'id'}} years old.
  {{!-- OR: gravatarurl email id=100 def='id'--}}
</script>
</body>
```
- partials
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  
  $(function(){
    var data = {name: "Ke", age:1
      stats:{
        "y":10,
        "n":5
      }
    };
    var str = $("#stats").html();
    Handlebars.registerPartial("stats",statTemplate);
    var str = $("#testTemplate").html();
    var template = Handlebars.compile(str);
    var result = template(data);
    $("#resdiv").html(result);
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {{age}} years old.
  {{> stats}}
</script>

<script id="stats" type="x-handlebars-template"
  <table>
    <tr>
      <td>{{stat.y}}</td>
      <td>{{stat.x}}</td>
    </tr>
  <table>
</body>
```

###3.Dust

- True False
```js
{?isTrue}
xxx
{/isTrue}



{^isFalse}
xxx
{/isFalse}

{?elsetest}
yes
{:else}
no
{/elsetest}
```

- put it together
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "Ke"};
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-dust-template">
  <h1>Hello {name}</h1>
</script>
</body>
```

- section data
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  
  $(function(){
    var data = {name: "Ke", age:1
      stats:{
        "y":10,
        "n":5
      }
    };
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {age} years old.
  <h2>Stats</h2>
  {#stats}
  <table>
    <tr>
      <td>y</td>
      <td>{y}</td>
    </tr>
    <tr>
      <td>n</td>
      <td>{n}</td>
    </tr>
  <table>
  {/stats}
</body>
```
- array dot syntax
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  
  $(function(){
    var data = {name: "Ke", age:1
    hobbies:["books","swimming"]
    };
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {age} years old.
  <h2>Stats</h2>
  <ul>
  {#hobbies}
    <li>{.}</li>
  {/hobbies}  
  </ul>
</body>
```
- array of objects
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  
  $(function(){
    var data = {name: "Ke", age:1
    hobbies:[{name:"books",level:"novice"},{name:"swimming",level:"pro"}]
    };
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-handlebars-template">
  You are {age} years old.
  <h2>Stats</h2>
  <ul>
  {#hobbies}
    <li>{name}{level}</li>
  {/hobbies}  
  </ul>
</body>
```

- filter
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">
  $(function(){
    var data = {name: "<strong>Ke</strong>"};
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-dust-template">
  <h1>Hello {name|h}</h1>
</script>
</body>
```

- custom filter
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">

dust.filter.titlecase = function(){
  return str.replace(/\w\S*/g, function(txt){return
  txt.chatAt(0).toUpperCase() + txt.substr(1).toLowerCase();});};
  
  $(function(){
    var data = {name: "book is mine"};
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-dust-template">
  <h1>Hello {name}</h1>
</script>
</body>
```

- context helper
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">

  
  $(function(){
    var data = {name: "book is mine",age:40,high:function(){
    return this.age>10;}};
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-dust-template">
  <h1>Hello {name}</h1>
  {#high}
    content....
  {/high}
</script>
</body>
```

- two level context
- 
```js
<body>
<div id="resdiv"></div>
<script type="text/javascript">

  
  $(function(){
    var data = {name: "book is mine",age:40,
    high:function(){
      return this.age>10;
    }
    describe: function(chunk,context, bodies,params){
    if(this.age<10) return true //simplest case
    if(this.age<10){
    return chunk.render(bodies.subOne, context);  //{:subOne} will show if this is true
    
    };
    var str = $("#testTemplate").html();
    var template = dust.compile(str,"strname");
    dust.loadSource(template);
    dust.render("strname",data,function(err,result){
      $("#resdiv").html(result);
    });
  });
</script>

<script id="testTemplate" type="text/x-dust-template">
  <h1>Hello {name}</h1>
  {#describe}
    {:subOne}
      xxxxx
    {:subTwo}
      xxxx
  {/describe}
</script>
</body>
```
