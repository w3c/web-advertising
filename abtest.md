```
<style>
.abc1,.abc2,.abc3 {
    display:none;
}
</style>
<script>
// check for existence of low-entropy (non user unique) ABTest item in localStorage.
// if absent then create one to show 3 ads 33% of the time each:
    var ABText = localStorage["ABText"];
    if(!ABText)
	    localStorage["ABText"] = Math.ceil(Math.random()*100).toString()+";Max-Age=5000000";
    var cls="adm3";
    if(ADText<33)
        cls= "adm1";
    if(ADText<66)
        cls= "adm2";
    var elem=querySelector(cls);
    elem.style.display = "inherit";
</script>
</style>
    <div class="ab1" ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=ab1;margin=5px;threshold=0.8;after=5000“>
        ... render ad 1
    </div>
    <div class="ab2" ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=ab2;margin=5px;threshold=0.8;after=5000“>
        ... render ad 2
    </div>
    <div class="ab3"  ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=ab3;margin=5px;threshold=0.8;after=5000“>
        ... render ad 3
    </div>
```