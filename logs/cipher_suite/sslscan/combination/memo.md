# final

SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA:!MD5
SSLHonorCipherOrder on

# draft

SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA:!SHA1:!MD5
SSLHonorCipherOrder on

# draft2

SSLProtocol all -SSLv3
SSLCipherSuite ALL:!LOW:!aNULL:!EXP:!3DES:!RC4:!IDEA
SSLHonorCipherOrder on

# now
SSLProtocol all -SSLv3
SSLCipherSuite ALL:!3DES:!ADH:!RC4:+HIGH:+MEDIUM:!LOW:!SSLv2:!EXP
SSLHonorCipherOrder on

