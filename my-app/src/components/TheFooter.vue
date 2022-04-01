<template>
  <div class="footer">
      <p>
        <small>This project is built with <strong>VueJs</strong> and <strong>Element Plus</strong>.</small>
      </p>
      <p>
        <small>
          View my code on
          <span>
            <a
              href="https://github.com/Qrious-Coder/vue-todo.git"
              target="_blank"
              rel="noopener noreferrer"
            >
              Github
            </a>
          </span>
          or drop me your valuable comments at any below:
        </small>
      </p>
      <p>
        <span class='footer-icon'>
          <a
            href="https://github.com/Qrious-Coder/vue-todo.git"
            target="_blank"
            rel="noopener noreferrer"
          >
            <img :src="gh" alt="github" width="30" height="30" />
          </a>
        </span>
        <span class='footer-icon'>
          <a
            href="https://mail.google.com/mail/?view=cm&source=mailto&to=[q81188118@gmail.com]"
            target="_blank"
            rel="noopener noreferrer"
          >
            <img :src="mail" alt="github" width="30" height="30" />
          </a>
        </span>
        <span class='footer-icon'>
          <a href="https://wa.me/639086471423" 
          target="_blank" 
          rel="noopener noreferrer">
            <img :src="tele" alt="github" width="30" height="30" />
          </a>
        </span>
        <span class='footer-icon'>
          <a href="https://t.me/Hillary_Duan" 
          target="_blank" 
          rel="noopener noreferrer">
            <img :src="whs" alt="email" width="30" height="30" />
          </a>
        </span>
      </p>
      <p>
        <small>Copyright © 2022 Q_coder. All Rights Reserved.</small>
      </p>
  </div>
</template>

<script>
import gh from '../assets/gh.png'
import mail from '../assets/mail.png'
import tele from '../assets/tele.png'
import whs from '../assets/whs.png'
  export default {
    name: 'TheFooter',
    setup(){
      return {
        gh,
        mail,
        tele,
        whs
      }
    }
  }
</script>

<style scoped>
.footer{
   left: 0;
   bottom: 0;
   height: 10vh;
   background-color: #f1a7fe;
   color: #6050dc;
   text-align: center;
   height: fit-content;
   font-size: 1rem;
   padding: 2px;
}
.footer-icon {
  margin: 0 2px;
  padding: 5px;
}

.footer-icon:hover {
  padding: 0 10px;
  border-bottom: purple solid 1px;
}

a:hover {
  color: purple;
  font-weight: bolder ;
}
</style>