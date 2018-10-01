# AWS Keys
#Cloud/AWS/Keys

* Convert a AWS PEM into a ssh pub key
`ssh-keygen -y -f private_key.pem > public_key.pub`

* If you want the DER encoded version of your PEM private key. So:
`openssl rsa -outform der -in private.pem -out private.key`

EC2에서 DSA 키는 허용되지 않는다. 키 생성기가 RSA 키를 만들도록 설정되어 있는지 확인한다.
지원되는 길이: 1024, 2048, 4096
(길이가 2048이 넘으니 오류가 발생했다. 따라서 길이는 그 이하로.)

