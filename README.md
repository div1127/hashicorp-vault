# Hashicorp Vault Setup
Vault is an open-source tool used for securely storing and managing secrets.

What is a secret? 

Secrets are securely-sensitive or personally identifiable info like database credentials, SSH keys, usernames and passwords, AWS IAM credentials, API tokens, Social Security Numbers, credit card numbers, just to name a few.

### Installation

Note:  You should install vault binary on the system from where you are going to interact with vault server or else you can access the vault service from the vault UI. Optionally, we can also work with the docker exec command for running the vault commands from inside the vault container.  

#####  This is only needed if you are going to interact from the DOCKER HOST terminal. 
Installation instructions of vault binary on the DOCKER HOST: \
https://learn.hashicorp.com/vault/getting-started/install 

Create the directory structure:
```
$ mkdir -p volumes/{config,file,logs} 
```
Export the vault address: (if you are going to interact from outside the container)
```
export VAULT_ADDR='http://127.0.0.1:8200'
```
Start the vault server:
```
docker-compose up -d
```

### Initialize the Vault Cluster:

###### Initialize new vault cluster with 6 key shares:

```
$ vault operator init -key-shares=6 -key-threshold=3
```
Output: 
```
Unseal Key 1: RntjR...DQv
Unseal Key 2: 7E1bG...0LL+
Unseal Key 3: AEuhl...A1NO
Unseal Key 4: bZU76...FMGl
Unseal Key 5: DmEjY...n7Hk
Unseal Key 6: pC4pK...XbKb

Initial Root Token: s.F0JGq..98s2U

Vault initialized with 10 key shares and a key threshold of 3. Please
securely distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 3 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
In order to unseal the vault cluster, we need to supply it with 3 key shares:

$ vault operator unseal RntjR...DQv
$ vault operator unseal bZU76...FMGl
$ vault operator unseal pC4pK...XbKb
```
#### Get vault status:

```
$ vault status -format=json
{
  "type": "shamir",
  "initialized": true,
  "sealed": false,
  "t": 3,
  "n": 5,
  "progress": 0,
  "nonce": "",
  "version": "1.1.0",
  "migration": false,
  "cluster_name": "vault-cluster-dca2b572",
  "cluster_id": "469c2f1d-xx-xx-xx-03bfc497c883",
  "recovery_seal": false
}
```

#### Authenticate against the vault.

```
$ vault login s.tdlEqsfzGbePVlke5hTpr9Um

Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.
Using the cli your auth token will be saved locally at ~/.vault-token.

OR 
export VAULT_TOKEN="s.tdlEqsfzGbePVlke5hTpr9Um"
Note: You would have to provide the unseal keys and root token on the web UI.
```
