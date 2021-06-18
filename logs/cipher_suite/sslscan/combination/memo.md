# draft

SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1
SSLHonorCipherOrder on
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA:!SHA1:!MD5

# draft2

SSLProtocol all -SSLv3
SSLHonorCipherOrder on
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA

# now
SSLProtocol all -SSLv3
SSLHonorCipherOrder on
SSLCipherSuite ALL:!3DES:!ADH:!RC4:+HIGH:+MEDIUM:!LOW:!SSLv2:!EXP

