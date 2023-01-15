# KeyAuth JS Module

## Code
```js
import * as KeyAuth from 'keyauth-js-module'
import readline from "readline";
import moment from "moment"

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
})

KeyAuth.api(
    "", // Application Name
    "", // OwnerID
    "", // Application Secret
    "1.0" // Application Version
)

await KeyAuth.cls();
await KeyAuth.init();

if (!KeyAuth.response.success) {
    KeyAuth.error("Status: " + KeyAuth.response.message)
}

KeyAuth.Title("KeyAuth JS Example - 1.1 API")
console.log("\n Application Data:")
console.log(` Number of users: ${KeyAuth.app_data.numUsers}`)
console.log(` Number of online users: ${KeyAuth.app_data.numOnlineUsers}`)
console.log(` Number of keys: ${KeyAuth.app_data.numKeys}`)
console.log(` Application Version: ${KeyAuth.app_data.version}`)
console.log(` Customer panel link: ${KeyAuth.app_data.customerPanelLInk}\n`)
await KeyAuth.check();
await KeyAuth.Sleep(1200);
console.log(` Current Session Validation Status: ${KeyAuth.response.message}`);
console.log(` Blacklisted: ${await KeyAuth.checkblacklist()}`);

await rl.question("\n [1] Login\n [2] Register\n [3] Upgrade\n [4] License key only\n\n Choose option: ", async function(choice) {
    if (choice === "1") {
        rl.question("Whats your username: ", async function(username) {
            rl.question("Whats your password: ", async function(password) {
                await KeyAuth.Login(username, password);
                if (!KeyAuth.response.success) {
                    KeyAuth.error("Status: " + KeyAuth.response.message)
                }
                loggedin();
                rl.close();
            })
        })
    } 
    else if (choice === "2") {
        rl.question("Whats your username: ", async function(username) {
            rl.question("Whats your password: ", async function(password) {
                rl.question("Whats your License: ", async function(key) {
                    await KeyAuth.Register(username, password, key);
                    if (!KeyAuth.response.success) {
                        KeyAuth.error("Status: " + KeyAuth.response.message)
                    }
                    loggedin();
                    rl.close();
                })
            })
        })
    }
    else if (choice === "3") {
        rl.question("Whats your username: ", async function(username) {
            rl.question("Whats your License: ", async function(key) {
                await KeyAuth.Upgrade(username, key);
                if (!KeyAuth.response.success) {
                    KeyAuth.error("Status: " + KeyAuth.response.message)
                }
                loggedin();
                rl.close();
            })
        })
    }
    else if (choice === "4") {
        rl.question("Whats your License: ", async function(key) {
            await KeyAuth.License(key);
            if (!KeyAuth.response.success) {
                KeyAuth.error("Status: " + KeyAuth.response.message)
            }
            loggedin();
            rl.close();
        })
    }
    else {
        console.log("?")
        rl.close();
    }
})

async function loggedin() {
    console.log("\n Logged In!")

    console.log(` Username: ${KeyAuth.user_data.username}`)
    console.log(` IP address: ${KeyAuth.user_data.ip}`)
    console.log(` Hardware-Id: ${KeyAuth.user_data.hwid}`)
    console.log(` Created at: ${moment.unix(KeyAuth.user_data.createdate).format("DD-MM-YYYY - HH:mm:ss")}`)
    console.log(` Last Login: ${moment.unix(KeyAuth.user_data.lastlogin).format("DD-MM-YYYY - HH:mm:ss")}`)


    for (var i = 0; i < KeyAuth.user_data.subscriptions.length; i++)
    {
        console.log(` [${i}] Subscription name: ${KeyAuth.user_data.subscriptions[i].subscription} | Expires at: ${moment.unix(KeyAuth.user_data.subscriptions[i].expiry).format("DD-MM-YYYY - HH:mm:ss")} | Time left in seconds ${KeyAuth.user_data.subscriptions[i].timeleft}`)
    }


    KeyAuth.check();
    console.log(` Current Session Validation Status: ${KeyAuth.response.message}`);

    console.log("\n\n Closing in 10 seconds...")
    await KeyAuth.Sleep(10000);
    process.exit(0);
}

```

### Credits
```md
# readline - https://www.npmjs.com/package/readline
# moment - https://www.npmjs.com/package/moment
# Axios - https://www.npmjs.com/package/axios
# Systeminformation - https://www.npmjs.com/package/systeminformation
# Querystring - https://www.npmjs.com/package/querystring
```

### Creator
mazkdevf - mazk9145

## Copyright License

KeyAuth is licensed under **Elastic License 2.0**

* You may not provide the software to third parties as a hosted or managed
service, where the service provides users with access to any substantial set of
the features or functionality of the software.

* You may not move, change, disable, or circumvent the license key functionality
in the software, and you may not remove or obscure any functionality in the
software that is protected by the license key.

* You may not alter, remove, or obscure any licensing, copyright, or other notices
of the licensor in the software. Any use of the licensor’s trademarks is subject
to applicable law.

Thank you for your compliance, we work hard on the development of KeyAuth and do not appreciate our copyright being infringed.