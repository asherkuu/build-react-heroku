# npm install --save compression express morgan nodemon

# Add proxy in package.json - "proxy" : "http://localhost:5000"

# Add Procfile - web: node server.js

# Project Directory

[Project]
--node_modules
--public
--src
-----App.js
-----Index.js
--.gitignore
--package-lock.json
--package.json
--Procfile
--README.md
--server.js

# Add server.js

const { createServer } = require("http");
const express = require("express");
const compression = require("compression");
const morgan = require("morgan");
const path = require("path");

const normalizePort = (port) => parseInt(port, 10);
const PORT = normalizePort(process.env.PORT || 5000);

const app = express();
const dev = app.get("env") !== "production";

app.get("/api/data", (req, res) => {
const json = { result: "true" };
res.send(json);
});

if (!dev) {
app.disable("x-powered-by");
app.use(compression());
app.use(morgan("common"));

    app.use(express.static(path.resolve(__dirname, "build")));

    app.get("*", (req, res) => {
        res.sendFile(path.resolve(__dirname, "build", "index.html"));
    });

}

if (dev) {
app.use(morgan("dev"));
}

const server = createServer(app);
server.listen(PORT, (err) => {
if (err) throw err;
console.log(`Server started !!`);
});
