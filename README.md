# test
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>比特幣匯率轉換</title>
		<link rel="stylesheet" href="myhomework.css">
	</head>
	
	<body>
		<div style="text-align:center">
			<h1><b>比特幣轉換美金小工具</b></h1>
		</div>
		
		
		<div style="text-align:center">
			<label>匯率日期:</label>
			<input id="date" type="date" name="date" min="2021-05-19" max="" oninput="validDate(this.value, this)" value=""/>
			<input id="BTC" type="text" name="num" oninput = "clearNoNum(this)" placeholder="請輸入數字 EX:100" value=""/><label>比特幣</label>
			<input id="btn" type="button" value="轉換"/>&nbsp;為&nbsp;&nbsp;<p id="result1"></p>美金
		</div>
		
		<br>
		
		<div style="text-align:center">
			(匯率於每日凌晨三點更新)
		</div>
		
		<br>
		
		<div>
			<table border="1" style="border: 3px #BBFFBB solid" align="center" width="70%">
				<thead style="background-color:#C1FFE4" height="100px">
					<tr>
						<td align='center' valign="middle">
							<h3><b>當前匯率</b></h3>
						</td>
					</tr>
				</thead>
				
				<tbody style="background-color:	#FBFFFD" height="100px">
					<tr>
						<td align='center' valign="middle">
							1比特幣約<p id="result2"></p>美金
						</td>
					</tr>
				</tbody>
			</table>
		</div>
		
		<script>
			var today = new Date();
			var dd = today.getDate();
			var mm = today.getMonth()+1; 
			var yyyy = today.getFullYear();
			if(dd<10){
				dd='0'+dd
			} 
			if(mm<10){
				mm='0'+mm
			} 

			today = yyyy+'-'+mm+'-'+dd;
			document.getElementById("date").setAttribute("max", today);
			
			function validDate(intodate, theInput) { 
            var intodate = document.getElementById("date").value; 
            if (intodate == today)
                theInput.value = yyyy+'-'+mm+'-'+(dd-1); 
            } 
		</script>
		
		<script>
            function clearNoNum(obj){
				obj.value = obj.value.replace(/[^\d.]/g,"");
				obj.value = obj.value.replace(/^\./g,"");
				obj.value = obj.value.replace(/\.{2,}/g,".");
				obj.value = obj.value.replace(".","$#$").replace(/\./g,"").replace("$#$",".");
			}
        </script>
		
			
		<script>
			document.getElementById("btn").onclick = function(){
			
				var indate = document.getElementById("date").value;
				var inBTC = parseFloat(document.getElementById("BTC").value);
				var request = new XMLHttpRequest();
				
				request.open('GET', 'https://api.coindesk.com/v1/bpi/historical/close.json',true);
				
				request.onreadystatechange = function (){
					if(this.readyState === 4 && this.status === 200){
						var jadata = JSON.parse(request.response);
						console.log(jadata);
						document.getElementById("result2").innerHTML = parseFloat(jadata['bpi'][indate]);
						document.getElementById("result1").innerHTML = parseFloat(jadata['bpi'][indate])*inBTC;
					}
				}
				request.send();
				
				
			}
		</script>
	</body>
</html>
