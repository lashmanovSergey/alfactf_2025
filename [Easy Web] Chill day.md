### Task "Chill day"

Difficulty: Easy

Category: Web

#### Analysis

There is against web-application that serves archivation of another web services. It works via link, so to archive any web-site you need to pass link. We are also given with code,
lets examine it for something interesting:
```go
func isHostAllowed(host string) bool {
	rrs, err := lookupHostWithCache(host)
	if err != nil {
		log.Println(err)
		return false
	}

	hasValidRecords := false
	for _, rr := range rrs {
		fmt.Println(rr.String())
		if rr.Header().Rrtype == dns.TypeA {
			// SECURITY: disallow metadata service
			if rr.Header().Name == "169.254.169.254" {
				return false
			} else {
				hasValidRecords = true
			}
		}
	}

	return hasValidRecords
}
```
We can see comment. It says to developers to disallow metadata service. So our first goal is to access server metadata.

#### Exploitation

##### step 1 (DNS rebinding to get metadata):
From the code fragment we understand that we cannot easily access metadata, because it checks for it. Because server has own proxy server to validate ip, we can try DNS rebinding attack.
I configured the following server, which always switches between google server and metadata server. The idea is simillar to race condition. I mean, we wanna pass proxy server with google 
ip and then switch ip to metadata and get it content:
<img width="627" height="75" alt="image" src="https://github.com/user-attachments/assets/16e189eb-3d2b-47f8-9236-59a7d2142956" />

We sent the following request:
<img width="823" height="325" alt="image" src="https://github.com/user-attachments/assets/a8c57004-b273-4db1-a15b-dd3a046c3883" />

We got the following response:
<img width="765" height="186" alt="image" src="https://github.com/user-attachments/assets/bd6b8004-d9e6-4a42-9cd7-0d721575c47a" />

It means, that we succesfully got access to the metadata!

##### step 2 (Information gathering and getting ssh):

After some time we can navigate through metadata and find credentials and public ip. We also know, that server has open ssh port => it's an idea to try connect via ssh.

And you will be success (but now servers are down, so I cannot show you this process:(( )

### Congrats!!
