const fetch = require('node-fetch');

//const typebotlink = 'http://192.168.8.4:3002';
const typebotlink = 'https://chat.funil-com-ia.com.br/';
const typebotid = 'cons-rcios-e-seguros-zff9d33';
const typebottoken = 'jnjRGVgw9LXTesjg3dyQB0h4';

async function iniciarchat(message){

    let url = `${typebotlink}/api/v1/typebots/${typebotid}/startChat`;
    
    let options = {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: `{"message":"${message}"}`
    };
    
    try {
        const response = await fetch(url, options);
        const json = await response.json();
        return json;
    } catch (err) {
        console.error('error:' + err);
        return { error: err };
    }
}

// testar a função iniciarchat
//iniciarchat('oi').then(response => console.log(response));

async function continuarchat(message, sessionId){

    let url = `${typebotlink}/api/v1/sessions/${sessionId}/continueChat`;
    
    let options = {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: `{"message":"${message}"}`
    };
    
    try {
        const response = await fetch(url, options);
        const json = await response.json();
        return json;
    } catch (err) {
        console.error('error:' + err);
        return { error: err };
    }
}




















const Insta = require("node-ig-framework");
const client = new Insta.Client();

client.on("connected", () => {
  console.log(`lolololololo`);
  console.log(`Logged in as ${client.user.fullName} (${client.user.username})`);
});

function convertRichTextToPlainText(richText) {
    const boldMap = {
        'a': '𝗮', 'b': '𝗯', 'c': '𝗰', 'd': '𝗱', 'e': '𝗲', 'f': '𝗳', 'g': '𝗴', 'h': '𝗵', 'i': '𝗶', 'j': '𝗷',
        'k': '𝗸', 'l': '𝗹', 'm': '𝗺', 'n': '𝗻', 'o': '𝗼', 'p': '𝗽', 'q': '𝗾', 'r': '𝗿', 's': '𝘀', 't': '𝘁',
        'u': '𝘂', 'v': '𝘃', 'w': '𝘄', 'x': '𝘅', 'y': '𝘆', 'z': '𝘇',
        'A': '𝗔', 'B': '𝗕', 'C': '𝗖', 'D': '𝗗', 'E': '𝗘', 'F': '𝗙', 'G': '𝗚', 'H': '𝗛', 'I': '𝗜', 'J': '𝗝',
        'K': '𝗞', 'L': '𝗟', 'M': '𝗠', 'N': '𝗡', 'O': '𝗢', 'P': '𝗣', 'Q': '𝗤', 'R': '𝗥', 'S': '𝗦', 'T': '𝗧',
        'U': '𝗨', 'V': '𝗩', 'W': '𝗪', 'X': '𝗫', 'Y': '𝗬', 'Z': '𝗭',
        '0': '𝟬', '1': '𝟭', '2': '𝟮', '3': '𝟯', '4': '𝟰', '5': '𝟱', '6': '𝟲', '7': '𝟳', '8': '𝟴', '9': '𝟵', 
    };

    let plainText = '';
    for (let block of richText) {
        if (block.type === 'p') {
            for (let child of block.children) {
                if (child.bold) {
                    // Convertendo o texto para negrito
                    plainText += [...child.text].map(char => boldMap[char] || char).join('');
                } else {
                    plainText += child.text;
                }
            }
            // Adicionando uma quebra de linha em cada elemento 'p'
            plainText += '\n';
        }
    }
    return plainText;
}
var relaçãomessage_author_id2typebotsession_id = {}


client.on("messageCreate", async (message) => {
  if (message.author.id === client.user.id) return;

  //verificar se o author_id já tem uma session_id
    if (relaçãomessage_author_id2typebotsession_id[message.author.id] == undefined){
        //se não tiver, iniciar uma nova conversa
        var response = await iniciarchat(message.content)
        
        // se a resposta for um erro, tenta novamente
        while (response.error != undefined){
            response = await iniciarchat(message.content)
        }
            
            
            relaçãomessage_author_id2typebotsession_id[message.author.id] = response.sessionId
  message.markSeen();

  let clientSideActions = response.clientSideActions || [];

  response.messages.reduce((promiseChain, msg, index) => {
    return promiseChain.then(() => {
      switch (msg.type) {
          case 'text':
              const text = convertRichTextToPlainText(msg.content.richText);
              return message.chat.sendMessage(text);
          case 'image':
              return message.chat.sendPhoto(msg.content.url);
              
          case 'video':
              return message.chat.sendMessage(msg.content.url);
          default:
              console.log(`Unsupported message type: ${msg.type}`);
              return Promise.resolve();
      }
    }).then(() => {
        // Check if there is a clientSideAction for this message
        let action = clientSideActions.find(a => a.lastBubbleBlockId === msg.id);
        if (action && action.type === 'wait') {
          // Start typing
          return new Promise(resolve => {
            message.chat.startTyping({ time: 5000 });
            resolve();
          }).then(() => {
            // Wait for the specified amount of time
            return new Promise(resolve => setTimeout(resolve, action.wait.secondsToWaitFor * 1000));
          });
        }
      });
    }, Promise.resolve());

    } else {
        //se tiver, continuar a conversa
        var response = await continuarchat(message.content, relaçãomessage_author_id2typebotsession_id[message.author.id])
// se a resposta for um erro, tenta novamente
        while (response.error != undefined){
            response = await continuarchat(message.content, relaçãomessage_author_id2typebotsession_id[message.author.id])
        }
        
    message.markSeen();

    let clientSideActions = response.clientSideActions || [];

    response.messages.reduce((promiseChain, msg, index) => {
      return promiseChain.then(() => {
        switch (msg.type) {
            case 'text':
                const text = convertRichTextToPlainText(msg.content.richText);
                return message.chat.sendMessage(text);
            case 'image':
                return message.chat.sendPhoto(msg.content.url);
            case 'video':
                return message.chat.sendMessage(msg.content.url);
            default:
                console.log(`Unsupported message type: ${msg.type}`);
                return Promise.resolve();
        }
      }).then(() => {
          // Check if there is a clientSideAction for this message
          let action = clientSideActions.find(a => a.lastBubbleBlockId === msg.id);
          if (action && action.type === 'wait') {
            // Start typing
            return new Promise(resolve => {
              message.chat.startTyping({ time: 5000 });
              resolve();
            }).then(() => {
              // Wait for the specified amount of time
              return new Promise(resolve => setTimeout(resolve, action.wait.secondsToWaitFor * 1000));
            });
          }
        });
      }, Promise.resolve());


    }
  });


  client.login("odonto.pel", "33551047pP!");

  //abre um servidor http para manter o bot rodando
  const http = require('http');
  const port = 7070;
  const requestHandler = (request, response) => {
    console.log(request.url)
    response.end('Hello Node.js Server!')
  }
  const server = http.createServer(requestHandler)
  server.listen(port, (err) => {
    if (err) {
      return console.log('something bad happened', err)
    }
  
    console.log(`server is listening on ${port}`)
  })
