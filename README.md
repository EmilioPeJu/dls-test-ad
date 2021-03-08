# DLS-TEST-AD
Nix derivation to create a test docker image

# How to build
* Install Nix (if you don't have it)
    ```bash
    $ curl -L https://nixos.org/nix/install | sh
    ```
* Make sure you have the latest nixpkgs channel
    ```bash
    $ nix-channel --add https://nixos.org/channels/nixpkgs-unstable
    ```
* Build the image from inside this project's folder, and load the result
    ```bash
    $ nix-build
    ...
    $ docker load < result
    ...
    ```

# Content
* EPICS Base R7.0.5 and required support modules
* Test Area Detector IOC (running with procServ using port 7011)
* Test Malcolm instance (running with procServ using port 7012)

# How to use this image
```bash
docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -h $(hostname -s) \
     -p 8008:8008 -p 6064:6064/tcp -p 6064:6064/udp -p 8080:8080 \
     -p 5075:5075/tcp -p 5076:5076/udp -p 6075:6075/tcp \
     -d dls-test-ad
```
If access to procServ is required, also expose port 7011 and 7012.
Alternatively, use the host network:
```bash
docker run  -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -h $(hostname -s) --network=host \
     -d dls-test-ad
```

Then, edm screens can accessed by running:
```bash
docker exec $CONTAINERID /bin/TS-EA-IOC-01-gui
```

