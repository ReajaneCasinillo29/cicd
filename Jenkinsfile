pipeline {
agent any
environment {

GIT_REPO_URL = &#39;https://github.com/calvinjohnplacio/testlab.git&#39;
GIT_CREDENTIALS_ID = &#39;github-pat2&#39;
GIT_BRANCH = &#39;main&#39;
}
stages {
stage(&#39;Checkout&#39;) {
steps {
checkout([$class: &#39;GitSCM&#39;, branches: [[name:
&quot;*/${env.GIT_BRANCH}&quot;]], userRemoteConfigs: [[url: &quot;${env.GIT_REPO_URL}&quot;,
credentialsId: &quot;${env.GIT_CREDENTIALS_ID}&quot;]]])
}
}
stage(&#39;Detect Change&#39;) {
steps {
script {
def changed = sh(script: &quot;git diff --name-only HEAD~1 HEAD | grep
&#39;.php&#39; | head -n 1&quot;, returnStdout: true).trim()
env.TARGET_PHP_FILE = changed ?: &quot;index.php&quot;
}
}
}
stage(&#39;Deploy&#39;) {
steps {
sh &#39;&#39;&#39;
# Only runs if test.py exited with 0
sudo rsync -av --delete --exclude=&#39;venv/&#39; --exclude=&#39;.git/&#39; --
exclude=&#39;staging/&#39; ./ /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
&#39;&#39;&#39;
}
}

}
}
