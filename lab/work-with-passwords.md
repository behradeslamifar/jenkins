# Mask/Hide Password in console logs

withCredentials plugin will mask password
```
withCredentials([usernamePassword(credentialsId: 'amazon', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
  sh 'echo $PASSWORD'
  // also available as a Groovy variable
  echo USERNAME
  // or inside double quotes for string interpolation
  echo "username is $USERNAME"
}
```

Hide password in sh block
```
node {
  withCredentials([usernameColonPassword(credentialsId: 'mylogin', variable: 'USERPASS')]) {
    sh '''
      set +x
      curl -u "$USERPASS" https://private.server/ > output
    '''
  }
}
```


wrap MaskPasswordsBuildWrapper class
```
node {
        wrap([$class: 'MaskPasswordsBuildWrapper', varPasswordPairs: [[password: '123ADS', var: 'SECRET']]]) {
        echo "123ADS";
    }
}
```


