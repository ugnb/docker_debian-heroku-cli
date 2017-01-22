## Debian slim with Heroku CLI (toolbelt) installed.

### Usage:

Pull container:
`docker pull ugnb/debian-heroku-cli`

Run container for the first time to login:
`docker run --name=heroku-cli -it ugnb/debian-heroku-cli`

Commit container with your login token to new local image and remove useless container:
`docker commit heroku-cli heroku-cli && docker rm heroku-cli`

Now you can use Heroku CLI:
`docker run --rm -it heroku-cli <Heroku command here>`


### Usage in Jenkins Pipeline:
Login to Heroku CLI locally and copy credentials from `.netrc` file.
```
node {
  withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Heroku',
                    usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
    stage('Build step that uses Heroku CLI') {
      docker.image('ugnb/debian-heroku-cli:latest').inside('-u 0:0') {
        sh 'printf "machine api.heroku.com\n\tlogin %s\n\tpassword %s\n" "$USERNAME" "$PASSWORD" > ~/.netrc'

        sh 'ANY_HEROKU_COMMAND_HERE'
      } 
    }
  }
} 
```
