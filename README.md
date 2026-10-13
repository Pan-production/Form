<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sketch show: get involved</title>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@400;600;800&display=swap" rel="stylesheet">
<style>
:root{--bg:#f1f3f8;--card:#fff;--ink:#14213d;--mute:#5a6480;--line:#cfd5e4;--accent:#b3263e;--on:#fff;--spot:#e9b44c;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#101626;--card:#19213a;--ink:#eef1fa;--mute:#a3acc6;--line:#2c3556;--accent:#e0566d;--on:#101626}}
:root[data-theme="dark"]{--bg:#101626;--card:#19213a;--ink:#eef1fa;--mute:#a3acc6;--line:#2c3556;--accent:#e0566d;--on:#101626}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font:17px/1.5 "Bricolage Grotesque",system-ui,sans-serif}
main{max-width:640px;margin:0 auto;padding:24px 18px 48px}
h1{font-size:clamp(34px,9vw,52px);line-height:1.02;font-weight:800;margin:8px 0 12px;letter-spacing:-.02em}
.lede{color:var(--mute);margin:0 0 28px}
.bar{height:6px;background:linear-gradient(90deg,var(--accent) 0 70%,var(--spot) 70%);border-radius:3px}
fieldset{border:0;padding:0;margin:0 0 28px}
legend,label.q{display:block;font-weight:600;margin-bottom:4px;padding:0}
.hint{color:var(--mute);font-size:15px;margin:0 0 10px}
input[type=text],textarea,select{width:100%;font:inherit;color:var(--ink);background:var(--card);border:1.5px solid var(--line);border-radius:10px;padding:12px}
textarea{min-height:90px;resize:vertical}
input:focus,textarea:focus,select:focus,button:focus-visible{outline:3px solid var(--spot);outline-offset:1px}
.opt{display:flex;gap:12px;align-items:center;padding:11px 12px;margin-bottom:8px;background:var(--card);border:1.5px solid var(--line);border-radius:10px;cursor:pointer}
.opt input{width:22px;height:22px;accent-color:var(--accent);flex:none;margin:0}
.opt:has(input:checked){border-color:var(--accent)}
button{font:inherit;font-weight:600;border:0;border-radius:12px;padding:15px 20px;background:var(--accent);color:var(--on);width:100%;cursor:pointer}
button.alt{background:transparent;color:var(--ink);border:1.5px solid var(--line);margin-top:10px}
#msg{margin-top:16px;padding:14px;border-radius:10px;background:var(--card);border-left:5px solid var(--spot)}
#msg.err{border-left-color:var(--accent)}
</style>
</head>
<body>
<main>
<div class="bar"></div>
<h1>Join our sketch show</h1>
<p class="lede">We're making a sketch comedy show for next year's comedy festival and need people for the stage and behind it. Tell us how you'd like to help. It takes about 3 minutes, and beginners are welcome.</p>

<fieldset><label class="q" for="name">Your name</label><input type="text" id="name" autocomplete="name"></fieldset>

<fieldset><legend>Which roles interest you?</legend><p class="hint">Tick as many as you like.</p><div id="roles"></div></fieldset>
<fieldset><label class="q" for="first">Which one is your first choice?</label><select id="first"><option value="">Choose a role</option></select></fieldset>

<fieldset><label class="q" for="exp">Relevant experience</label><p class="hint">Comedy, theatre, production, anything. No experience is fine too.</p><textarea id="exp"></textarea></fieldset>
<fieldset><label class="q" for="perf">If you'd like to perform: what have you done, and what characters or styles suit you?</label><p class="hint">Sketch, improv, stand-up, acting, impressions.</p><textarea id="perf"></textarea></fieldset>

<fieldset><legend>When can you do rehearsals?</legend><div id="avail"></div></fieldset>
<fieldset><label class="q" for="block">Dates you can't do between now and the festival</label><textarea id="block"></textarea></fieldset>
<fieldset><label class="q" for="hrs">Hours a week you can realistically give</label><select id="hrs"><option value="">Choose one</option><option>1-2 hours</option><option>3-5 hours</option><option>6 or more hours</option></select></fieldset>
<fieldset><label class="q" for="pay">Is unpaid work okay for you?</label><select id="pay"><option value="">Choose one</option><option>Yes, unpaid is fine</option><option>I'd prefer paid</option><option>Either, let's talk</option></select></fieldset>
<fieldset><label class="q" for="more">Anything else we should know?</label><p class="hint">Skills, equipment, contacts, access needs.</p><textarea id="more"></textarea></fieldset>

<button id="send">Send my answers</button>
<button class="alt" id="copy">Copy my answers</button>
<div id="msg" hidden role="status"></div>
</main>
<script>
var TO="anpanart@gmail.com";
var ROLES=["Performer","Assistant director","Stage manager","Tech operator (lights and sound)","Props and costumes","Marketing and social media","Photo or video","Script feedback","Other"];
var AVAIL=["Weekday mornings","Weekday evenings","Weekends"];
function opts(box,name,list){list.forEach(function(t){var l=document.createElement("label");l.className="opt";var i=document.createElement("input");i.type="checkbox";i.name=name;i.value=t;l.appendChild(i);l.appendChild(document.createTextNode(t));box.appendChild(l);});}
var rolesBox=document.getElementById("roles"),first=document.getElementById("first");
opts(rolesBox,"role",ROLES);opts(document.getElementById("avail"),"avail",AVAIL);
ROLES.forEach(function(t){var o=document.createElement("option");o.textContent=t;first.appendChild(o);});
function v(id){return document.getElementById(id).value.trim()||"(not answered)";}
function checked(n){return Array.prototype.map.call(document.querySelectorAll("input[name="+n+"]:checked"),function(x){return x.value;});}
function msg(t,err){var m=document.getElementById("msg");m.hidden=false;m.className=err?"err":"";m.textContent=t;}
function build(){
var nm=document.getElementById("name").value.trim(),r=checked("role");
if(!nm){msg("Add your name.",true);return null;}
if(!r.length){msg("Pick at least one role you're interested in.",true);return null;}
var a=checked("avail");
return {subject:"Sketch show: "+nm+" wants to help",body:["Name: "+nm,"Roles: "+r.join(", "),"First choice: "+(first.value||"(not answered)"),"Experience: "+v("exp"),"Performing background: "+v("perf"),"Rehearsal times: "+(a.length?a.join(", "):"(not answered)"),"Dates I can't do: "+v("block"),"Hours per week: "+v("hrs"),"Unpaid okay: "+v("pay"),"Anything else: "+v("more")].join("\n\n")};
}
document.getElementById("send").onclick=function(){
var d=build();if(!d)return;
window.location.href="mailto:"+TO+"?subject="+encodeURIComponent(d.subject)+"&body="+encodeURIComponent(d.body);
msg("Your email app should open with your answers filled in. Press send there to finish. If nothing opened, use Copy my answers and email them to "+TO+".");
};
document.getElementById("copy").onclick=function(){
var d=build();if(!d)return;
try{navigator.clipboard.writeText(d.body).then(function(){msg("Copied. Paste your answers into an email to "+TO+".");},function(){msg("Copying was blocked. Use Send my answers instead.",true);});}
catch(e){msg("Copying was blocked. Use Send my answers instead.",true);}
};
</script>
</body>
</html>
