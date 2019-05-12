```
<style>
.abc1,.abc2,.abc3 {
    display:none;
}
</style>
<script>
// check for existence of low-entropy ABTest cookie.
// if absent then create one to show 3 ads 33% of the time each:
    var ADText = getCookie("ABTest");
    if(!ADText)
        putCookie("ABTest=Math.ceil(Math.random()*100).toString()+";Max-Age=5000000");
    var cls="adm3";
    if(ADText<33)
        cls= "adm1";
    if(ADText<66)
        cls= "adm2";
    var elem=querySelector(cls);
    elem.style.display = "inherit";
</script>
</style>
    <div class="ab1">
        ... render ad 1
    </div>
    <div class="ab2">
        ... render ad 2
    </div>
    <div class="ab3">
        ... render ad 3
    </div>
```