

<p style="text-align: center;">

	<script type="text/javascript" src="//t1.daumcdn.net/adfit/static/ad.min.js" async></script></p>			
	<table style="width: 95%;">
	<tbody><tr>
		<td>
			<input type="radio" name="unix_radio" value="Encoding" checked="">UNIX타임을 일반시간으로
		</td>
	</tr>
	<tr>
		<td><textarea id="unix_url" style="width: 100%;height: 100px; line-height:1.2; resize:none;padding:10px;" placeholder="이곳에 변환하실 UNIX시간을 넣어주세요."></textarea></td>
	</tr>
	<tr>
		<td><input type="button" class="button" value="변환" id="unix_change">
		    <input type="button" class="button" value="초기화" id="unix_trash">
		    <input type="button" class="button" value="복사" id="unix_copy"> <b style="color:red;">복사버튼은 현재 크롬에서만 지원가능합니다.</b>
		</td>
	</tr>
	<tr>
		<td><textarea id="unix_result" style="width: 100%; height: 100px; line-height:1.2; resize:none; padding:10px;" placeholder="상단의 변환버튼을 클릭하시면 이곳에 출력됩니다."></textarea></td>
	</tr>
</tbody></table><br />

<script>
<!-- document.ready -->

 document.getElementById('unix_change').onclick= function(){
      unix_change();
   }

 document.getElementById('unix_trash').onclick= function(){
      unix_trash();
   }

document.getElementById('unix_copy').onclick= function(){
      unix_copy();
   }
<!-- document.ready -->
function unix_change(){
  
  
	var data = document.getElementById("unix_url").value;
  
  if(data.trim() == ""){
    var now =  new Date();
    var de_time = now.getTime() / 1000;
    
    document.getElementById("unix_url").value = de_time;
    data = de_time;
  }
  
  if(isNaN(data)){
     alert('숫자만 변경가능합니다.');
     unix_trash();
     return;
  }
  
	var timestamp = data * 1000;
  var date = new Date(timestamp);
  
  var re_year = date.getFullYear();//year
  var re_month = date.getMonth()+1;//month
  //if(re_month == 0){
  //  re_year = Number(re_year) -1;
  //  re_month =12;
  //}
  var re_day = date.getDate();//day
  var re_hours  = date.getHours();//hour
  var re_min = date.getMinutes()//minutes
  var re_seconde = date.getSeconds();//seconde
  
  var de = ["-","-", " ", ":",":"," "];
  var kor = ["년","월", "일", "시","분","초"];
  var joon = ["年","月","日","时","分","秒"];
  var time = [re_year, re_month, re_day, re_hours,re_min, re_seconde];
  
  
  var result_de ="";
  for(var i =0; i <= kor.length-1; i++){
    result_de += time[i]+ de[i];
  }
  
  var result_kor ="";
  for(var i =0; i <= kor.length-1; i++){
    result_kor += time[i]+ kor[i]+" ";
  }
  
  var result_joon ="";
  for(var i =0; i <= kor.length-1; i++){
    result_joon += time[i]+ joon[i]+" ";
  }

  
  var temp = result_de +"\n"+result_kor +"\n"+result_joon;
		document.getElementById("unix_result").value = temp;
	}
	function unix_trash(){
		document.getElementById("unix_url").value = "";
		document.getElementById("unix_result").value = "";
	}
	
	function unix_copy(){
		 var copy = document.getElementById("unix_result");
		 copy.select();
		 var result = document.execCommand("copy");
               
	}
</script>
