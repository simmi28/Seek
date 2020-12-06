# Seek
const { connect } = require('http2');

var app = require('express')();
var http = require('http').createServer(app);
var io = require('socket.io')(http);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

var currentUsers = []
const allowedUserSize = [0, 1]

function validateUser(userId) {
  if (allowedUserSize.includes(currentUsers.length)){
    currentUsers.push(userId)
  }
}

io.on('connection', (socket) => {
  socket.on('chat message', (msg) => {
    console.log(msg)
    const messageObj = JSON.parse(msg)
    io.emit('chat message', messageObj.message);
  });
});

http.listen(3000, () => {
  console.log('listening on *:3000');
});
