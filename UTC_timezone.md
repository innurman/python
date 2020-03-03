#현재 날짜를 넣으면 유닉스 시간 알려 줌

https://heavenly-appear.tistory.com/260


https://brownbears.tistory.com/242

      from time import localtime, strftime

      now = 1407694710
      local_tuple = localtime(now)
      time_format = '%Y-%m-%d %H:%M:%S'
      time_str = strftime(time_format, local_tuple)
      print(time_str)

      # 결과
      # 2014-08-11 03-18-30

       from time import localtime, strftime, mktime, strptime

      now = 1407694710
      local_tuple = localtime(now)
      time_format = '%Y-%m-%d %H:%M:%S'
      time_str = strftime(time_format, local_tuple)
      time_tuple = strptime(time_str, time_format)
      utc_now = mktime(time_tuple)
      print(utc_now)

      # 결과
      # 1407694710.0
