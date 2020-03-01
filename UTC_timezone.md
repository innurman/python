https://brownbears.tistory.com/242

      from time import localtime, strftime

      now = 1407694710
      local_tuple = localtime(now)
      time_format = '%Y-%m-%d %H:%M:%S'
      time_str = strftime(time_format, local_tuple)
      print(time_str)

      # 결과
      # 2014-08-11 03-18-30

 
