# AWS
#Cloud/AWS/EC-2/commands

pem 파일, 그리고 EC2의 public ip를 알고 있다면, 아래와 같은 커맨드로 접속할 수 있다. 

먼저 아래와 같은 방법으로 pem 파일의 권한을 변경해준다.
`chmod 400 ~/Desktop/key/keyfile.pem`

`ssh -i "wacaKeySeoul.pem" ec2-user@52.79.153.72`

#Cloud/AWS/EC-2
키페어 정책을 추가하거나 수정하고 싶다면, 아래의 파일을 수정하면 된다.
`$ vim .ssh/authorized_keys`

파일을 열면 아래와 같은 내용이 등장한다.
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCDljRYMIuQyVl6xEqyfik0rhE2bS3fK1/SLYazvxs2jrmu2GFjh46u31qJaYcdmw4m9YCJJZpUPyVbcGQuuT05QsjUvaP1aTrMtrMtIQrgyB/syNWJuyBEnbL8e5vXLXn6epJD2tZhgZZmxrtVY7BxFZAmti++zuyZcKpvJJv7CTS+2y18esDaApD/2NOI+5mB5eaIPTfVQ87WsBy/1jUUt+bF1l/o/yxpUBCJTGUjeKtWJpybS3cxmmDo6J+BAO99nNK7tVNnWNPGJhR/MC+hqJBGw8zSCKeZ1ZWwpYMdVRbTNFfKgvbyf4xwmKqoEA9YGqdW17WjX5T9sbo7TThl qqw
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC8ad9+S+RFAYOVZET4rsETB74K2j34uF5BfdcTNLUczA93wvzwj/gPNYb+1SoxX4hikpfQ/OMEfT2eLbN+2xffo4KcKueJMso/FhYVErrSu0LxeLwHM+l5u4qAPyClvk6xpiR6UMtDKPzoH5KBAy17zuI6iiuUmFrsLBh+HJwNMz6qxGvrl0UXwlBcmUQIHVp4Qsjs1Jxb6e+l6p66MbTtWo2G/AWo5I7iU4PjmOeP02AU7hJB+tjBdYECh39AYy9jWY/gYjbJO26KeyTGf2SXkqJbxw260BNCEAK9Nc+0eXTDbS6Ojkm8iAnjOitUzlNxG0FN7MhG8KxKhmcbGZKD hyunwoo
```

키페어를 여러개 생성하여 사용할 수도 있고 기존의 키페어 내용을 수정할 수도 있다.

키페어를 잃어버리거나 하는 상황 등에는 아래의 사이트를 참조하여 해결하도록 한다.

[Amazon EC2 Key Pairs - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#replacing-lost-key-pair)
