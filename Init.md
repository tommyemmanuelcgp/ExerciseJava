**Tạo repository**

1. Đăng nhập github, chọn New repository
----------------------------------------

![](media/968075a6c93352edfa709943c3faad6f.jpg)

Trong phần New repository, đặt tên Repository, chọn Add.gitignore là java và sau
đó Creat repository

![](media/92c8753f616a66e76997c9ab8392632a.jpg)

2. Tạo project
--------------

Mở IntelliJ chọn File -\> New -\> Project

Sau đó chọn Project kiểu Gradle và tick vào Java trong Addtional Libraries and
Framworks

![](media/dec0ae91c97a09813a012253d42493e7.jpg)

Tiếp tục điền tên ứng dụng

Chú ý: Riêng phần Location(tích vào icon bên phải) hãy chọn ổ nào đó để lưu đừng
lấy ổ C nghen.Ở trong bước này là ổ D, sau đó tạo một thư mục tên là code. Làm y
như hình là được

![](media/e35d27de519135348c2604af438d96a0.jpg)

3. Add sample code
------------------

Sau khi dự án được tạo, đợi một lúc để IDE đồng bộ cấu hình gradle từ template,
ta sẽ được cái quần què này.Ai KHÔNG bị cái này thì bỏ qua nghen(cái lỗi dưới
đây chỉ có Windows 10 thôi ai mà Windows 7 thích thì nghe không thích thì nghe)
ta chọn Fix

![](media/83e6f899aabd14467e36a54b90bc0518.jpg)

Bấm xong nó sẽ ra như hình dưới chọn Configure Automatically

![](media/65ce54e0ffadfc541e175433a083ad40.jpg)

Rồi tiếp tục chọn Allow access

![](media/e34a812928c2bd9617bd36c70e0668fb.jpg)

Ta thêm class Demo vào gói main/java

![](media/48f57ab729e17b08f8269f27aa2956e0.jpg)

Với nội dung

public class Demo {

public int sum(int x, int y){

return x+y;

}

public int sub(int x, int y){

return x-y;

}

public int multiple(int x, int y){

return x\*y;

}

}

Thêm class DemoTest vào gói test/java

![](media/a5597dcae106e8fba238ea0381bca8e4.jpg)

Với nội dung

import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class DemoTest {

\@Test

public void test_sum(){

assertEquals(new Demo().sum(2, 8), 10);

}

\@Test

public void test_sub(){

assertEquals(new Demo().sub(8, 3), 5);

}

\@Test

public void test_multiple(){

assertEquals(new Demo().multiple(8, 3), 24);

}

}

Thêm xong nó ra thế này

![](media/d0645830c1972c46dc943ab48332c527.jpg)

![](media/3b0d35524e1b4a35b19a4858f643c463.jpg)

Chạy DemoTest

![](media/88f795f2ae75c46d5c05ab9b2ceb630b.jpg)

Nếu kết quả nhận được toàn màu xanh là done

![](media/88158b1f6c4df61f9330f10e632cfcbb.png)

4. Cấu hình CircleCI và Codecov
-------------------------------

Mở file build.gradle (xem mấy cái ở trên để biết cái file nó nằm ở đâu mà mở
nghen)

Thay

plugins {

id 'java'

}

ở đầu file thành

apply plugin: 'java'

apply plugin: 'jacoco'

Thêm đoạn này vào cuối file

jacocoTestReport {

reports {

xml.enabled true

html.enabled false

}

}

check.dependsOn jacocoTestReport

Và kết quả như ảnh dưới đây

![](media/2f12e779e9f8b23787176a3751ffcf3c.jpg)

Tạo một thư mục tên là .circleci (nhấn chuột phải vào exercise chọn New -\>
Directory) và sau đó tạo 1 file tên là config.yml

![](media/e713dbce4e652188505967c1701817ca.jpg)

với nội dung

\# Java Gradle CircleCI 2.0 configuration file

\#

\# Check https://circleci.com/docs/2.0/language-java/ for more details

\#

version: 2

jobs:

build:

docker:

\# specify the version you desire here

\- image: circleci/openjdk:8-jdk

\# Specify service dependencies here if necessary

\# CircleCI maintains a library of pre-built images

\# documented at https://circleci.com/docs/2.0/circleci-images/

\# - image: circleci/postgres:9.4

working_directory: \~/repo

environment:

\# Customize the JVM maximum heap limit

JVM_OPTS: -Xmx3200m

TERM: dumb

steps:

\- checkout

\# Download and cache dependencies

\- restore_cache:

keys:

\- v1-dependencies-{{ checksum "build.gradle" }}

\# fallback to using the latest cache if no exact match is found

\- v1-dependencies-

\- run: gradle dependencies

\- save_cache:

paths:

\- \~/.gradle

key: v1-dependencies-{{ checksum "build.gradle" }}

\# run tests!

\- run:

name: Running test

command: gradle test

\# Upload test coverage

\- run:

name: Generate test coverage

command: gradle jacocoTestReport

\# Upload test coverage

\- run:

name: Upload test coverage

command: bash \<(curl -s https://codecov.io/bash) \|\| echo "Codecov did not
collect coverage reports"

5. Đồng bộ project với github repository
----------------------------------------

Mở command line tại thư mục gốc của dự án(thư mục gốc là exercise chứ không phải
là code) và gõ git init

![](media/e1623e88b1cb4eb8591d608a8576253a.jpg)

![](media/b8d3b76c841f7e6bde08d0a9afd557b0.jpg)

Copy url của repository như hình (ở hình dưới là repository của mình còn các bạn
lấy cái repository của các cậu nghen)

![](media/f29c60c7adb65aa1b5cc8f1ce8f2cd11.jpg)

Rồi chạy lệnh git remote add origin \<url vừa copy\>

![](media/23cee8d090a7bb2b238782065513d020.jpg)

Tiếp tục chạy git pull để lấy code về

![](media/b1eb1e0108aee65618147230f46f5dcb.jpg)

Qua bên IntelliJ IDEA chọn Always Add

![](media/39abe0a0434159e09fcebdb3cec30f63.jpg)

Mở file .gitignore (Nếu mà không có file thì chạy tiếp git pull origin master

![](media/6698e3f5a41509a1c6d1ce868c9d80b4.jpg)

Thêm các dòng này vào cuối file

build/\*\*

.gradle/\*\*

.idea/\*\*

out/\*\*

![](media/d7732ab315d1f57bbff32c4222f51d21.jpg)

Sau đó đẩy code lên(chạy lần lượt và Enter từng dòng)

git add .circleci/config.yml

git add \*

git commit -m Init

git push --set-upstream origin master

Chú ý: Khi chạy dòng git commit -m Init

(Nếu không hiện như hình dưới thì bỏ còn nếu hiện thì làm theo dưới đây)

![](media/99193a610189925907096c3530fce39c.jpg)

Chạy lần lượt 2 cái lệnh git config nó đã gợi ý như hình, riêng trong phần dấu
ngoặc kép có thể đổi tên như ý muốn hoặc không .Ví dụ như

git config user.name "someone"

git config user.email "someone\@someplace.com"

Chạy lại git commit -m Init

Sau đó hiện ra

![](media/0f3c8a011238687e62034915e161c6d6.jpg)

Nếu đang thực hiện các lệnh trên đó tự nhiên hiện cái Login Github thì cứ đăng
nhập vào thôi

![](media/ca4946050cd28f68f3854c2ea0ee600f.jpg)

6. Build code bằng circleCI
---------------------------

Đăng nhập circle ci trên trang https://circleci.com/vcs-authorize/, chọn phần
Add projects

![](media/ce2b3e3756a3c71d5b61a6337072ebfa.jpg)

Tìm repository và bấm Set Up Project. Sau đó chọn kiểu dự án là Gradle trên
Linux

![](media/ffb71feca7965414e44c050e151c7359.jpg)

Sau đó bấm Start Building rồi bấm tiếp Start Building lần nữa (Nhớ thắp hương
trước khi bấm).Đợi nó chạy đã.

Nếu bạn gặp may, nó sẽ xanh lè như thế này

![](media/97fdc01a4c79903711ba6a497a5cae4b.jpg)

Sau đó tìm chổ Project setting (bánh răng cưa phía trên bên phải), copy mã Embed
Code trong phần Status Badges (cụ thể là Markdown) vào đâu đó đã, ở bước cuối ta
sẽ tạo file Readme.md

![](media/b1efa1d2908e51bc84bb185264565a56.jpg)

7. Cấu hình Codecov
-------------------

Vào trang [https://codecov.io](https://codecov.io/), đăng nhập bằng tài khoản
github sau đó chọn repository hiện ra.

![](media/94e0f26d188382cc2fd3917da2cb5998.jpg)

Sau đó vào setting (Bánh răng cưa phía trên bên phải )

![](media/8f1dd697662e4ab508e65621a6790470.jpg)

Chọn Badge và copy phần mã Markdown

![](media/9988fda0d773e20020d417affa90accc.jpg)

8. Tạo file Readme.md
---------------------

Vào Visual Studio Code chọn File -\> New File

![](media/07122eecc2101c398fc2bf8818d9547e.jpg)

Với nội dung như hình dưới

![](media/b0285cdd0780942265490a0aa0db14b0.jpg)

Sau đó lưu file này ở thư mục gốc dự án đặt tên file là Readme và Save as type
là Markdown.Cuối cùng là Save

![](media/2b41d3af4a926a899f20f66f50ebce09.jpg)

Đầy file này lên

![](media/f6458619446cac42847d2c7d75a96e77.jpg)

Tận hưởng thành quả

![](media/37a97acc1f02daef341d24d2f58d033f.jpg)

Done!!
======
