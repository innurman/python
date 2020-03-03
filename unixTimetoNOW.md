1 유닉스 시간을 현재의 시간으로 변경 	

	<p style="text-align: center;">

		<script type="text/javascript" src="//t1.daumcdn.net/adfit/static/ad.min.js" async></script></p>			
		<table style="width: 95%;">
		<tbody><tr>
			<td>
				<input type="radio" name="unix_radio" value="Encoding" checked="">UNIX타임을 일반시간으로
			</td>
		</tr>
		<tr>
			<td>
 	<textarea id="unix_url" style="width: 100%;height: 100px; line-height:1.2; resize:none;padding:10px;" placeholder="이곳에 변환하실 UNIX시간을 넣어주세요.">
	</textarea></td>
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



2 현재 시간을 유닉스 시간으로

	<p style="text-align: center;">

		<script type="text/javascript" src="//t1.daumcdn.net/adfit/static/ad.min.js" async></script></p>			

	<p><b style="color:red;">잠시만 기다려주시면 밑의 박스에 시간이 로딩됩니다.</b></p>
	<fieldset>
	   <legend>년,월,일,시,분,초의 시간을 입력하면 자동으로 유닉스 시간을 계산해 줍니다.</legend>
	<p>
	  <select id="u_year"></select> 년(year)
	  <select id="u_month"></select> 월(month)
	  <select id="u_day"></select> 일(day)
	</p>
	<p>
	  <select id="u_hour"></select> 시(hour)
	  <select id="u_minutes"></select> 분(minutes)
	  <select id="u_seconde"></select> 초(seconde)
	</p>
	</fieldset>
	<p>
	 <input type="button" class="button" value="변환" id="u_change">  
	  <input type="button" class="button" value="초기화" id="u_trash">
	 <input type="button" class="button" value="복사" id="u_copy"> <b style="color:red;">복사버튼은 현재 크롬에서만 지원가능합니다.</b>
	</p>
	<p>
	<textarea id="u_result" style="width: 100%; height: 100px; line-height:1.2; resize:none; padding:10px;" placeholder="상단의 변환버튼을 클릭하시면 이곳에 출력됩니다."></textarea>
	</p>

	<script>
	<!-- document.ready -->

	  var year = document.getElementById("u_year");

	  var top_year = 2100;
	  var t_year_temp = "";
	  for(var i = 1970; i <= top_year ; i++){
	    t_year_temp += "<option value='"+i+"'>"+i+"</option>";
	  }
	  year.innerHTML = t_year_temp;
	  //--------------year

	  var month = document.getElementById("u_month");
	  var top_month = 12;
	  var t_month_temp = "";
	  for(var i = 1; i <= top_month ; i++){
	    t_month_temp += "<option value='"+i+"'>"+i+"</option>";
	  }
	  month.innerHTML = t_month_temp;

	  //------------------------month

	  var day = document.getElementById("u_day");
	  var top_day = 31;
	  var t_day_temp = "";
	  for(var i = 1; i <= top_day ; i++){
	    t_day_temp += "<option value='"+i+"'>"+i+"</option>";
	  }
	  day.innerHTML = t_day_temp;
	  //------------------------day


	  var hour = document.getElementById("u_hour");
	  var top_hour = 23;
	  var t_hour_temp = "";
	  for(var i = 0; i <= top_hour ; i++){
	    t_hour_temp += "<option value='"+i+"'>"+i+"</option>";
	  }
	  hour.innerHTML = t_hour_temp;

	  //----------------------------hour

	  var minutes = document.getElementById("u_minutes");
	  var top_minutes = 59;
	  var t_minutes_temp = "";
	  for(var i = 0; i <= top_minutes ; i++){
	    t_minutes_temp += "<option value='"+i+"'>"+i+"</option>";
	  }
	  minutes.innerHTML = t_minutes_temp;
	  //---------------------------------------minutes

	   var seconde = document.getElementById("u_seconde");
	  var top_seconde = 59;
	  var t_seconde_temp = "";
	  for(var i = 0; i <= top_seconde ; i++){
	    t_seconde_temp += "<option value='"+i+"'>"+i+"</option>";
	  }
	  seconde.innerHTML = t_seconde_temp;

	  u_trash();

	 document.getElementById('u_change').onclick= function(){
	      u_change();
	   }

	 document.getElementById('u_trash').onclick= function(){
	      u_trash();
	   }

	document.getElementById('u_copy').onclick= function(){
	      u_copy();
	   }
	<!-- document.ready -->

	function u_trash(){
	  var date = new Date();

	  var re_year = date.getFullYear();//year
	  var re_month = date.getMonth()+1;//month
	  var re_day = date.getDate();//day
	  var re_hours  = date.getHours();//hour
	  var re_min = date.getMinutes()//minutes
	  var re_seconde = date.getSeconds();//seconde

	  var target = document.getElementById("u_year");
	  //target.options[target.selectedIndex].text = re_year;
	  target.value=re_year;

	  target = document.getElementById("u_month");
	  //target.options[target.selectedIndex].text = re_month;
	  target.value=re_month;

	  target = document.getElementById("u_day");
	  //target.options[target.selectedIndex].text = re_day;
	  target.value=re_day;

	  target = document.getElementById("u_hour");
	  //target.options[target.selectedIndex].text = re_hours;
	  target.value=re_hours;

	  target = document.getElementById("u_minutes");
	  //target.options[target.selectedIndex].text = re_min;
	  target.value=re_min;

	  target = document.getElementById("u_seconde");
	  //target.options[target.selectedIndex].text = re_seconde;
	  target.value=re_seconde;
	 document.getElementById("u_result").value= "";
	}

	function u_change(){
	  var target = document.getElementById("u_year");
	  var year = target.options[target.selectedIndex].value;

	  target = document.getElementById("u_month");
	  var month = target.options[target.selectedIndex].value;

	  target = document.getElementById("u_day");
	  var day = target.options[target.selectedIndex].value;

	   target = document.getElementById("u_hour");
	  var hour = target.options[target.selectedIndex].value;

	   target = document.getElementById("u_minutes");
	  var minutes = target.options[target.selectedIndex].value;

	  target = document.getElementById("u_seconde");
	  var seconde =target.options[target.selectedIndex].value;


	  var d = new Date(year, month-1, day, hour, minutes, seconde);
	  var de_time = d.getTime() / 1000;

	  document.getElementById("u_result").value = de_time;
	}



	function u_copy(){
	      var copy = document.getElementById("u_result");
	      copy.select();
	   var result = document.execCommand("copy");
	   // window.clipboardData.setData("Text", copy.value);
	}
	</script>
