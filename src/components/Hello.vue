<template>
  <div class="hello">
<div class="mdl-grid">
  <div class="mdl-cell mdl-cell--4-col"></div>
  <div class="mdl-cell mdl-cell--4-col">
    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
    <input class="mdl-textfield__input" type="email" id="txtEmail" v-model="authInput.txtEmail">
    <label class="mdl-textfield__label" for="txtEmail">Email</label>
    </div>
    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
    <input class="mdl-textfield__input" type="Password" id="txtPassword" v-model="authInput.txtPassword">
    <label class="mdl-textfield__label" for="txtPassword">Password</label>
    </div>
    <button id="btnLogin" v-on:click="Login()" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--primary">
      Log in
    </button>
    <button id="btnSignUp" v-on:click="SignUp()"  class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent">
      Sign Up
    </button>
    <button id="btnLogout" v-on:click="LogOut()" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button" style="display:none">
      Log out
    </button>

    <h4>Login with</h4>

    <button v-on:click="googleLogin()" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button">
      <img class="a12n-provider-icon" ng-src="//www.gstatic.com/mobilesdk/160512_mobilesdk/auth_service_google.svg" src="//www.gstatic.com/mobilesdk/160512_mobilesdk/auth_service_google.svg"> Google
    </button>
    <button v-on:click="githubLogin()" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button">
      <img class="a12n-provider-icon" ng-src="//www.gstatic.com/mobilesdk/160409_mobilesdk/images/auth_service_github.svg" src="//www.gstatic.com/mobilesdk/160409_mobilesdk/images/auth_service_github.svg"> Github
    </button>
  </div>
  <div class="mdl-cell mdl-cell--4-col"></div>
</div>

  </div>
  </div>
</template>

<script>
    import Firebase from 'firebase'
    const config = {
      apiKey: "AIzaSyBZvJmFu8mrlliWmsYxfmO4UAb05wzpDZ8",
      authDomain: "wordfire-251e1.firebaseapp.com",
      databaseURL: "https://wordfire-251e1.firebaseio.com",
      storageBucket: "wordfire-251e1.appspot.com",
      messagingSenderId: "436757678533"
    }
    const app = Firebase.initializeApp(config);
    var googleAuthProvider = new Firebase.auth.GoogleAuthProvider();
    var githubAuthProvider = new Firebase.auth.GithubAuthProvider();
    export default {
        name: 'hello',
        data() {
            return {
                authInput: {
                    txtEmail: '',
                    txtPassword: ''
                }
            }
        },
        methods: {
            Login: function(event) {
                const email = this.authInput.txtEmail;
                const pass = this.authInput.txtPassword;
                const auth = Firebase.auth();
                const promise = auth.signInWithEmailAndPassword(email, pass);
                this.authInput.txtEmail = '';
                this.authInput.txtPassword = '';
                promise.catch(event => console.log(event.message));
            },
            SignUp: function(event) {
                const email = this.authInput.txtEmail;
                const pass = this.authInput.txtPassword;
                const auth = Firebase.auth();
                const promise = auth.createUserWithEmailAndPassword(email, pass);
                this.authInput.txtEmail = '';
                this.authInput.txtPassword = '';
                promise.catch(event => console.log(event.message));

            },
            googleLogin: function() {
                Firebase.auth().signInWithPopup(googleAuthProvider).then(function(result) {
                    console.log(result);
                    // Returns "send Verification"
                    // Tell user to check gmail account.
                    // Give link to gmail.com
                }).catch(function(error) {});
            },
            githubLogin: function() {
                Firebase.auth().signInWithPopup(githubAuthProvider).then(function(result) {
                    console.log(result);
                    // Not seeing anything.
                }).catch(function(error) {});
            },
            LogOut: function() {
                Firebase.auth().signOut();
            }
        }

    }
    Firebase.auth().onAuthStateChanged(firebaseUser => {
        if (firebaseUser) {
            firebaseUser.sendEmailVerification().then(function() {
                console.log('send Verification');
            }, function(error) {
                console.log('not send Verification');
            });
            if (firebaseUser.emailVerified == true) {
                document.getElementById('btnLogout').style.display = '';
            } else {
                document.getElementById('btnLogout').style.display = 'none';
            }


        } else {
            console.log('not loggend in');
            document.getElementById('btnLogout').style.display = 'none';
        }
    })
</script>
