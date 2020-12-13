
node app
test me
test


# generate your private key, put the public key on the server you will be connecting to
ssh-keygen -t rsa -f ./my_key

# generate the password/secret you will store encrypted in the .travis.yml and use to encrypt your private key
cat /dev/urandom | head -c 10000 | openssl sha1 > ./secret

# encrypt your private key using your secret password
openssl aes-256-cbc -pass "file:./secret" -in ./my_key -out ./my_key.enc -a

openssl aes-256-cbc -pass "pass:$MY_SECRET_ENV" -in ./my_key.enc -out ./my_key -d -a