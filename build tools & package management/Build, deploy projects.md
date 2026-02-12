# Deployment projects
This is instruction to build, deploy different projects in different languages.


## Java 

**Select java verion in terminal**
```bash
sudo update-alternatives --config java
sudo update-alternatives --config javac

```



```
mvn install -DskipTests=true
nohup java -jar target/filename.jar 2>&1 &

tail -f nohup.out

cp -rf /projects/shoeshop/* /data/shoeshop# Java
```

### Dealing with different Java, Maven and Gradle versions

SDK use SDK man
Build tools
	using Gradle and Maven wrapper
## Nodejs

| Command     | Description             |
| ----------- | ----------------------- |
| npm start   | start application       |
| npm stop    | stop application        |
| npm test    | run test                |
| npm publish | publish the artifact    |
| npm pack    | package code in to .tar |
### Build tools - Webpack

Webpack

## Notes
- Zip/tar file only include code, **NO** dependencies
- 