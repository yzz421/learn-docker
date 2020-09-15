## Our Application

At this point, your development team is quite small and you're simply building an app to prove out your MVP (minimum viable product). You want to show how it works and what it's capable of doing without needing to think about how it will work for a large team, multiple developers, etc.

![Todo List Manager Screenshot](todo-list-sample.png)



## Getting out App

Before we can run the application, we need to get the application source code onto our machine. For real projects, you will typically clone the repo. But, for this tutorial, we have created a ZIP file containing the application.

1. [Download the ZIP](/assets/app.zip). Open the ZIP file and make sure you extract the contents.
2. Once extracted, use your favorite code editor to open the project. If you're in need of an editor, you can use Visual Studio Code. You Should see the ```package.json``` and tow subdirectories (```src``` and ```spec```).

![Screenshot of Visual Studio Code opened with the app loaded](ide-screenshot.png)



## Building the App's Container Image

In order to build the application, we need to use a ```Dockerfile```. A Dockerfile is simply a text-based script of instructions that is used to create a container image. If you've created Dockerfiles before, you might see few flaws in the Dockerfile below.

1. Create a file named  ```Dockerfile``` in the same folder as the file ```package.json``` with the following contents.  

   ```dockerfile
   FROM node:12-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   ```

   Please check that the file ```Dockerfile``` has no file extension like ```.txt```. Some editors may append this file extension automatically and this would result in an error in the next step.

2. If you haven't already done so, open a terminal and go to the `app` directory with the `Dockerfile`. Now build the container image using the `docker build` command.

   `` `` docker build -t getting-started .

   This command used the Dockerfile to build a new container image. You might have noticed that a lot of "layers" were downloaded. This is because we instructions the builder that we wanted to start form the `node:12-alpine` image. But, since we didn't have that on our machine, that image needed to be downloaded.

   After the iamge wa downloaded, we copied in out application and used `yarn` to install our application's dependencies. The `CMD` directive specifies the default command to run when starting a container from this image.

   Finally, the `-t` flag tags our image. Think of this simply as a human-readable name for the final image. Since we named the image `getting-started`, we can refer to that iamge when we run a container.

   The `.` at the end of the `docker build` command tells that Docker should look for the `Dcokerfile` in current directory

## Starting an App Container

Now that we have an image, let't run the application! To do so, we will use the `docker run` command .

1. Start your container using the `docker run` command and specify the name of the image we just created:

   `docker run -dp 3000:3000 getting-started`

   `-d` and `-p` , means running the new container in "detached" mode (in the background) and creating a mapping between the host's port 3000 to the container's port 3000. Without the port mapping, we wouldn't be able to access the application.

2. After a few seconds, open your web browser to [http://localhost:3000](http://localhost:3000). You should see our app! 

   ![Todo List Manager Screenshot](todo-list-sample.png)

3. Go ahead and add an item or two and see that it works as you expect.

