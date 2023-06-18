## Contributing to this collection

Contribution to the collection requires the following steps as prepare:  
Ansible collection expects specific path for the collection to be placed in.
* Create the following path structure to contain the collection:
```bash
mkdir -p $TARGET_DIR/ansible/collections/ansible_collections/stolostron
cd !$
```
* Clone the collection repository and change its name:
```bash
git clone git@github.com:stolostron/ansible-collection.rhacm.git rhacm
```
* Export `ANSIBLE_COLLECTIONS_PATHS` to specify collection location:
```bash
export ANSIBLE_COLLECTIONS_PATHS=$TARGET_DIR/ansible/collections/
```
* Proceed with the development and testing
