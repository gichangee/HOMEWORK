20184474 박기창 오픈SW 과제 
=============
**목차**

1) [getopts](https://github.com/gichangee/HOMEWORK#1-getopts) 
2) [getopt](https://github.com/gichangee/HOMEWORK#2-getopt) 
   1) [getopts vs getopt](https://github.com/gichangee/HOMEWORK/blob/master/README.md#getopt-vs-getopts)
4) [sed](https://github.com/gichangee/HOMEWORK#3-sed) 
5) [awk](https://github.com/gichangee/HOMEWORK#4-awk) 


## 1) getopts

* ### getopts?

  주어진 옵션에 값을 저장할 수 있게 도와주는 명령어이다.

* ### getopts 사용하기
  ```bash
  while getopts a:e:c opt 
  do
      echo "opt=$opt, OPTARG=$OPTARG"
  done
  ```  
  
  opt는 옵션을 저장하는 변수\
  OPTARG는 옵션안에 들어있는 값을 저장하는 변수이다
  
  option을 정의할 때 뒤에 ':' 이 있으면 option안에 값을 넣을 수 있다\
  ex) a:
  
  a와e는 값을 넣을 수 있지만 c는 넣을 수 없다
* ### getopts 사용할때 주의점      
  1) a 와 e 옵션에 값을 넣지 않으면 오류가 발생한다 그러므로 반드시 값을 넣어줘야 한다. 
  
     ./main.py -a       ---> 오류 발생\
     ./main.py -a 123   ---> 정상
     
  2) 지정한 옵션 즉 a,e,c를 제외한 옵션이 들어왔을 때에도 오류가 발생
  
     ./main.py -z       ---> 오류 발생
      
  
  
  
      
  
      
  
* ### 쉘스크립트에서 직접 만들고 활용하기

  ```bash
  #!/bin/bash 

  while getopts a:e:c:d opt 
  
  do   
      case $opt in     
        a)
          A=$OPTARG
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
 ![캡처](https://user-images.githubusercontent.com/93646339/142749055-0f2124eb-81a8-4584-9e44-8d25731a73b1.PNG)


      
## 2) getopt

   * ### getopt?
     getopts는 짧은 옵션 밖에 사용하지 못하는 반면에 \
     getopt는 긴 옵션과 짧은 옵션을 둘다 활용 할 수 있는 명령어이다


   * ### getopt 사용하기  
     긴 옵션 --> -l\
     짧은 옵션 --> -o

     ```bash
     options="$(getopt -o a:e:c:d -l apple:,exam:,cute:,dia -- "$@")"
     eval set -- "$options"
     ```

     짧은 옵션을 지정할 때에는 getopts option 지정할때와 똑같이 하면 되지만 \
     긴 옵션을 지정할 때에는 옵션을 ,(콤마)를 사용하여 구분지어야하며\
     getopt 마지막에는 -- "$@" 를 붙여줘야 한다.
      
   * ### getopt 사용할때 주의점
     1) 지정하지 않은 옵션을 넣었을 때 오류 발생
      
        ./main.py -r          --->오류 발생\
        ./main.py --r         --->오류 발생
        
     2) 값이 필요한 옵션에 값을 넣지 않았을때 오류 발생 
     
        ./main.py -a          --->오류 발생\
        ./main.py --apple     --->오류 발생\
        ./main.py --apple 123 --->정상\
        ./main.py -a 123      --->정상


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
                    shift 2;; #옵션과 옵션안에 있는 값 총 2개의 값을 옮겨야 하기 때문에 shift 2를 해줘야 한다
           -e|--exam)
                     echo "$2"
                     shift 2;;
           -c|--cute)
                     echo "$2"
                     shift 2;;
           -d|--dia)
                     echo "hello world"
                     shift ;; #옵션만 있기 때문에 shift만 해주면 된다.
            --)
                     shift
                     break ;;
           esac
     done

     echo "$@"
     ```

   * **실행결과**
     ![실행결과](https://user-images.githubusercontent.com/93646339/142440973-b0feccbf-7371-465e-ac3c-b7a272a28f28.PNG)
     
     * #### eval?
         eval은 문자열을 명령어로 인식하는 명령어이다.
         
         c의 파일내용이 아래와 같다고 가정
          ```bash
          a="ls -al"
          echo "$a"
          eval "$a"
          ```

         **실행결과**
         ![실행결과](https://user-images.githubusercontent.com/93646339/142441803-453d3db7-74b0-4ecc-be7d-7501749c0765.PNG)
  
   * ### getopt vs getopts

      ||getopt|getopts|
      |:---:|:---:|:---:|
      |옵션 안에 값을 넣을 수 있는가?|O|O|
      |지정되지 않은 option을 사용했을 때 오류가 발생하는가?|O|O|
      |값이 필요한 option에 값을 넣지 않았을 때 오류가 발생하는가?|O|O|
      |긴 옵션을 사용 가능한가?|X|O|
      |뒤에 -- "$@" 구문을 붙여야 하는가?|O|X|


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
    #d는 삭제를 의미
    #s는 교체를 의미
    #a는 해당 문자열 뒤에 추가를 의미
    #i는 해당 문자열 앞에 추가를 의미
    
    sed -n -e '1p' -e '5p' main.py             ---> 첫번째 줄과 다섯번째 줄만 출력
    sed -n -e '1,3d' -e '1,$p' main.py         --->전체에서 1~3줄을 제외하고 출력
    sed -n -e 's/1/a/g' -e '1,$p' main.py      --->모든 1를 a로 바꾸고 출력
    sed -n -e '/1234/a\aaa' -e '1,$p' main.py  --->모든 1234문자열 뒤에 aaa 추가하고 출력
    sed -n -e '/1234/i\aaa' -e '1,$p' main.py  --->모든 1234문자열 앞에 aaa 추가하고 출력
    ```
   
## 4) awk

* ### awk?
  awk는 하나 이상의 공백으로 구분되어진 여러 열의 텍스트 데이터를 처리 할때  명령어이다.
 
* ### awk 사용하기

     ```bash
     awk '{print $1}' main.py #공백을 기준으로 필드를 나눈다
     #또는
     cat main.py | awk '{print $1}'
     ```
     main.py 파일 안의 내용이 아래와 같다고 가정\
     1\
     1 2\
     1 2 3\
     1 2 3 4\
     1 2 3 4 5 

     아래의 명령어를 실행하면

     ```bash
     awk '{print $1}' main.py # main.py의 공백을 기준으로 나누어진 첫번째 필드를 출력하라는 의미
     awk '{print "first num: " $1}' main.py #앞에 추가문구를 추가하여 출력
     
     ```

     출력 결과
     ```
     1
     1
     1
     1
     1
     first num: 1
     first num: 1
     first num: 1
     first num: 1
     first num: 1
     ```
* ### awk 활용법
  1) 구분값을 변경하기
  
      -F를 사용하여서 공백이 기준이 아닌 다른 것을 기준으로 삼을수도 있다\
       F뒤에 기준을 삼고싶은 기호를 쓰면 된다.\
       awk -F, ---> (,)를 기준으로 필드를 나눈다.\
       awk -F: ---> (:)를 기준으로 필드를 나눈다.

       main.py 파일 안의 내용이 아래와 같다고 가정\
       1:2:3\
       4:5:6

       아래의 명령어를 실행하면
       ```bash
       awk -F:'{print $1}' main.py #main.py의 ':'을 기준으로 나누어진 첫번째 필드를 출력하라는 의미
       ```

       출력결과
       ```
       1
       4
       ```
      
  2) 행 번호 추가하기
  
      print 옆에 NR 옵션을 추가하면 된다

      main.py 파일 안의 내용이 아래와 같다고 가정\
      1:2:3\
      4:5:6

      아래의 명령어를 실행하면
      ```bash
      awk -F:'{print NR " "$0}' main.py #main.py의 전체 텍스트를 앞에 행번호를 추가하여 출력하라
      #$0은 전체를 출력한다는 뜻
      ```
      출력결과
      ```
      1 1:2:3
      2 4:5:6
      ```

  3) 사칙연산 사용해보기
  
      main.py 파일 안의 내용이 아래와 같다고 가정\
      100\
      200\
      300

      아래의 명령어를 실행하면
      ```bash
      awk '{print $1+5}' main.py                  #첫번째 필드에 5를 더한값을 출력
      
      awk '{sum=sum+$1}END{print sum}' main.py    #합계 계산
      
      awk '{sum=sum+$1}END{print sum/NR}' main.py #평균 계산
      ```

      실행결과
      ```
      105
      205
      305
      
      600
      
      200
      ```


  4) 특정 문자열이 들어있는 행만 출력

        main.py 파일 안의 내용이 아래와 같다고 가정\
        a b c\
        a p p\
        c b a

        ```bash
        awk '$1=="a"' main.py # 첫번째 필드에 "a"가 있는 행만 출력
        
        awk '/a/' main.py     # 전체 텍스트 중에 a가 포함되어 있는 행 출력

        awk '$1!="a"' main.py # 첫번째 필드에 "a"가 없는 행만 출력
        ```

        실행결과
        ```
        a b c
        a p p
        
        a b c
        a p p
        c b a

        c b a
        ```
    
    
    
  
  
   
  
   
   
   
  
   
