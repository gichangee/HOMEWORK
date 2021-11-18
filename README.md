# 20184474 박기창 오픈SW 과제 
## 1) getopts

* ### getopts?

  주어진 옵션에 값을 저장할수 있게 도와주는 명령어이다.
  
* ### getopts 사용하기
  
  while getopts a:e:c opt \
  do
  
      echo "opt=$opt, OPTARG=$OPTARG"
  done
  
  option을 정의할 때 뒤에 : 이 있으면 option안에 값을 넣을 수 있다 ex) a:
  
  a와e는 값을 넣을 수 있지만 c는 넣을 수 없다
      
  이때 a 와 e 옵션에 값을 넣지 않으면 오류가 발생한다 그러므로 반드시 값을 넣어줘야 한다. 
       지정한 옵션 즉 a,e,c를 제외한 옵션이 들어왔을 때에도 오류가 발생
  
  ./main.py -a       ---> 오류 발생\
  ./main.py -z       ---> 오류 발생\
  ./main.py -a 123   ---> 정상
      
  opt는 옵션을 저장하는 변수\
  OPTARG는 옵션안에 들어있는 값을 저장하는 변수이다
      
  
* ### 쉘스크립트에서 직접 만들고 활용하기
  
  #!/bin/bash 

  while getopts a:e:c:d opt 
  
  do 
       
      case $opt in     
        a)
          A=$OPTAGR
          echo "$A";;
        e)
          E=$OPTARG
          echo "$E";;
        c)
          C=$OPTARG
          echo "$C";;
        d)
          echo "hello world";;
        *)
          echo "올바른 option 값을 넣으시오";;
     
      esac   
      
   done
 
 **실행결과**
 ![실행결과](https://user-images.githubusercontent.com/93646339/142413567-49b3b6b0-3a58-461b-ac7a-9cd9d7e54050.PNG)

      
## 2) getopt

* ### getopt?
  긴옵션과 짧은 옵션을 둘다 활용할수있는 명령어이다
  
  
* ### getopt 사용하기  
  긴 옵션 --> -l\
  짧은 옵션 --> -o
  
  options=$(getopt -o a:e:c:d -l apple:,exam:,cute:,dia -- $@)
  
  
  짧은 옵션을 지정할 때에는 getopts option 지정할때와 똑같이 하면 되지만 \
  긴 옵션을 지정할 때에는 옵션을 ,(콤마)를 사용하여 구분지어야하며\
  마지막에는 -- "$@" 를 붙여줘야 한다.
  
  지정하지 않은 옵션을 넣었을 때 오류 발생
  값이 필요한 옵션에 값을 넣지 않았을때 오류 발생
  
  ./main.py -r       --->오류 발생\
  ./main.py --r      --->오류 발생\
  ./main.py -a       --->오류 발생\
  ./main.py --apple  --->오류 발생
  
  
  
  
* ### 쉘스크립트에서 직접 만들고 활용하기  
  


## 3) sed

## 4) awk
