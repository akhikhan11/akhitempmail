<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Akhi Temp Mail</title>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 30px; }
    button { padding: 10px 20px; margin: 10px; font-size: 16px; }
    #emails { margin-top: 20px; }
  </style>
</head>
<body>
  <h1>Akhi Temp Mail</h1>
  <button onclick="generateEmail()">Generate Email</button>
  <p id="email"></p>
  <button onclick="checkInbox()">Check Inbox</button>
  <div id="emails"></div>

<script>
let email = '';

function generateEmail() {
  const chars = 'abcdefghijklmnopqrstuvwxyz1234567890';
  let randomStr = '';
  for (let i = 0; i < 8; i++) randomStr += chars[Math.floor(Math.random() * chars.length)];
  email = `${randomStr}@1secmail.com`;
  document.getElementById('email').innerText = email;
}

function checkInbox() {
  if (!email) return alert('Please generate an email first!');
  const [login, domain] = email.split('@');
  fetch(`https://www.1secmail.com/api/v1/?action=getMessages&login=${login}&domain=${domain}`)
    .then(res => res.json())
    .then(data => {
      let out = '<h3>Inbox:</h3>';
      if (data.length === 0) out += '<p>No mails yet.</p>';
      else data.forEach(mail => {
        out += `<p>📧 <strong>${mail.from}</strong>: ${mail.subject} <button onclick="readMail(${mail.id})">Read</button></p>`;
      });
      document.getElementById('emails').innerHTML = out;
    });
}

function readMail(id) {
  const [login, domain] = email.split('@');
  fetch(`https://www.1secmail.com/api/v1/?action=readMessage&login=${login}&domain=${domain}&id=${id}`)
    .then(res => res.json())
    .then(data => {
      alert(`From: ${data.from}\nSubject: ${data.subject}\n\n${data.body}`);
    });
}
</script>
</body>
</html>
# akhitempmail
