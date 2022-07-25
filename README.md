# azdo-repo-migrate-docker
Migrate repo from one Azure DevOps org to another using Docker


3 HOW
Using Docker to do a ```bare clone``` into a directory, then ```push``` to a different git origin.

Code can be found in ```src/go.sh``` file:

Essentially,

```
SRC_PAT=Blah blah blah
SRC_ORG="Org Name"
SRC_PROJNAME="Project"
SRC_REPO="repo name"
CLONE_FOLDER="$SRC_REPO-Folder1"


DEST_PAT=$SRC_PAT
DEST_ORG="Org name"
DEST_PROJNAME="Project"
DEST_REPO="Repo name"

SRC_URI=https://asdf:$SRC_PAT@dev.azure.com/$SRC_ORG/$SRC_PROJNAME/_git/$SRC_REPO
DEST_URI=https://asdf:$DEST_PAT@dev.azure.com/$DEST_ORG/$DEST_PROJNAME/_git/$DEST_REPO

mkdir $CLONE_FOLDER
cd $CLONE_FOLDER

echo "Cloning dev.azure.com/$SRC_ORG/$SRC_PROJNAME/_git/$SRC_REPO"
docker run -t --rm -v /:/root/ -v $(pwd):/git alpine/git:latest clone --bare $SRC_URI $CLONE_FOLDER

echo "Changing the ownership"
docker run -it --rm -v /:/root/ -v $(pwd)/$CLONE_FOLDER/:/git alpine/git:latest config --global --add safe.directory /git
echo "Mirring the repo.."
docker run -it --rm -v /:/root/ -v $(pwd)/$CLONE_FOLDER/:/git alpine/git:latest push --mirror $DEST_URI

```
