20184474 박기창 오픈SW 과제 
=============
## 1) getopts

* ### getopts?

  주어진 옵션에 값을 저장할수 있게 도와주는 명령어이다.

* ### getopts 사용하기
  ```  
  while getopts a:e:c opt 
  do
      echo "opt=$opt, OPTARG=$OPTARG"
  done
  ```  
  option을 정의할 때 뒤에 : 이 있으면 option안에 값을 넣을 수 있다\
  ex) a:
  
  a와e는 값을 넣을 수 있지만 c는 넣을 수 없다
      
  이때 a 와 e 옵션에 값을 넣지 않으면 오류가 발생한다 그러므로 반드시 값을 넣어줘야 한다. \
  지정한 옵션 즉 a,e,c를 제외한 옵션이 들어왔을 때에도 오류가 발생
  
  ./main.py -a       ---> 오류 발생\
  ./main.py -z       ---> 오류 발생\
  ./main.py -a 123   ---> 정상
      
  opt는 옵션을 저장하는 변수\
  OPTARG는 옵션안에 들어있는 값을 저장하는 변수이다
      
  
* ### 쉘스크립트에서 직접 만들고 활용하기
  ```bash
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
   ```
 * **실행결과**
 ![실행결과](https://user-images.githubusercontent.com/93646339/142413567-49b3b6b0-3a58-461b-ac7a-9cd9d7e54050.PNG)

      
## 2) getopt

* ### getopt?
  긴옵션과 짧은 옵션을 둘다 활용할수있는 명령어이다
  
  
* ### getopt 사용하기  
  긴 옵션 --> -l\
  짧은 옵션 --> -o
  ```
  options="$(getopt -o a:e:c:d -l apple:,exam:,cute:,dia -- "$@")"
  eval set -- "$options"
  ```
  
  짧은 옵션을 지정할 때에는 getopts option 지정할때와 똑같이 하면 되지만 \
  긴 옵션을 지정할 때에는 옵션을 ,(콤마)를 사용하여 구분지어야하며\
  getopt 마지막에는 -- "$@" 를 붙여줘야 한다.
  
  지정하지 않은 옵션을 넣었을 때 오류 발생\
  값이 필요한 옵션에 값을 넣지 않았을때 오류 발생
  
  ./main.py -r          --->오류 발생\
  ./main.py --r         --->오류 발생\
  ./main.py -a          --->오류 발생\
  ./main.py --apple     --->오류 발생\
  ./main.py --apple 123 --->정상\
  ./main.py -a 123      --->정상
  
  
   * #### eval?
      eval은 문자열을 명령어로 인식하는 명령어이다.
  
       ```
       a="ls -al"
       echo "$a"
       eval "$a"
       ```
          
      **실행결과**
      ![실행결과](https://user-images.githubusercontent.com/93646339/142441803-453d3db7-74b0-4ecc-be7d-7501749c0765.PNG)
   
  
  
* ### 쉘스크립트에서 직접 만들고 활용하기  
  ```bash
  #!/bin/bash
  
  options="$(getopt -o a:e:c:d -l apple:,exam:,cute:,dia -- "$@")"
  
  eval set -- "$options"
  
  while true
  
  do
 
       case $1 in
    
        -a|--apple)
                 echo "$2"
                 shift 2;;
        -e|--exam)
                  echo "$2"
                  shift 2;;
        -c|--cute)
                  echo "$2"
                  shift 2;;
        -d|--dia)
                  echo "hello world"
                  shift ;;
         --)
                  shift
                  break ;;
        esac
  done
  
  echo "$@"
  ```
  
* **실행결과**
  ![실행결과](https://user-images.githubusercontent.com/93646339/142440973-b0feccbf-7371-465e-ac3c-b7a272a28f28.PNG)
  ---
* **getopt,getopts 비교**
    
    
   
    ||getopt|getopts|
    |:---:|:---:|:---:|
    |지정되지 않은 option을 사용했을 때 오류가 발생하는가?|O|O|
    |값이 필요한 option에 값을 넣지 않았을 때 오류가 발생하는가?|O|O|
    |긴 옵션을 사용 가능한가?|X|O|


## 3) sed

* ### sed?
  원본파일안에 있는 내용을 변경시키지 않고 \
  텍스트 파일 내용을 출력,수정,추가,제거 등을 할수있게 해주는 명령어이다.

* ### sed 사용하기
  ```bash
  sed -n -e '1,$p' 파일이름
  ```
  |대표적인 옵션|정의|
  |:---:|:---:|
  |-n| 출력 범위 지정 |
  |-e| 다양한 출력 범위를 나타내고 싶을 때 사용|
  
  * sed -n 사용하기
  
    main.py 파일 안의 내용이 아래와 같다고 가정\
    1\
    12\
    123\
    1234\
    12345
    
    
    **쉘 스크립트에서 직접 해보기**
    
    ![캡처](https://user-images.githubusercontent.com/93646339/142604609-15e706eb-f8b1-46f5-b0e5-200c4660b116.PNG)
    
    ```bash
    #p는 print의 약자로 출력을 의미
    sed -n '1p' main.py      #첫번째 줄만 출력
    sed -n '1,3p' main.py    #첫번째 줄부터 세번째 줄까지 출력
    sed -n '1,$p' main.py    #처음부터 끝까지 출력
    sed -n' 1p;3p' main.py   #첫번째 줄과 세번째 줄만 출력
    ```

  * sed -e 사용하기
  
    main.py 파일 안의 내용이 아래와 같다고 가정\
    1\
    12\
    123\
    1234\
    12345

    **쉘 스크립트에서 직접 해보기**
    
    ![캡처](https://user-images.githubusercontent.com/93646339/142621949-0d3f73e4-77d2-4eca-a89e-8dd85b213c25.PNG)

    ```bash
    sed -n -e '1p' -e '5p' main.py             ---> 첫번째 줄과 다섯번째 줄만 출력
    sed -n -e '1,3d' -e '1,$p' main.py         --->전체에서 1~3줄을 제외하고 출력
    sed -n -e 's/1/a/g' -e '1,$p' main.py      --->모든 1를 a로 바꾸고 출력
    sed -n -e '/1234/a\aaa' -e '1,$p' main.py  --->모든 1234문자열 뒤에 aaa 추가하고 출력
    sed -n -e '/1234/i\aaa' -e '1,$p' main.py  --->모든 1234문자열 앞에 aaa 추가하고 출력
    ```
   
## 4) awk

* ### awk?
 
* ### awk 사용하기
