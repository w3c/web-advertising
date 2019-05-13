```
<style>
.abc1,.abc2,.abc3 {
    display:none;
}
</style>
<script>
// check for existence of low-entropy (non user unique) ABTest item in localStorage.
// if absent then create one to show 3 ads 33% of the time each:
    var now = Date.now();
    var ABText = localStorage["ABText"];
	var expires = localStorage["ABTextExpires"];
	if(!ABText)
	{
		localStorage["ABText"] = Math.ceil(Math.random()*100).toString();
		localStorage["ABTextExpires"] = expires = (now.setDate(now.getDate() + 500000).toISOString();
	}
// if past sell-by date delete it and do not show ad
	if(expires<now)
	{
		localStorage.removeItem("ABText");
		localStorage.removeItem("ABTextExpires");
	}
	else
	{
		var cls="abc3";
		if(ADText<33)
			cls= "abc1";
		if(ADText<66)
			cls= "abc2";
		var elem=document.querySelector(cls);
		if(elem)
			elem.style.display = "inherit";
	}
</script>


    <div class="abc1" ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=ab1;margin=5px;threshold=0.8;after=5000“>
        ... render ad 1
    </div>
    <div class="abc2" ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=ab2;margin=5px;threshold=0.8;after=5000“>
        ... render ad 2
    </div>
    <div class="abc3"  ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=ab3;margin=5px;threshold=0.8;after=5000“>
        ... render ad 3
    </div>
```