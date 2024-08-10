<template>
  <div class="terminal-container">
    <div class="terminal">
      <div class="terminal-header">
        <div class="header-buttons">
          <span class="button close"></span>
          <span class="button minimize"></span>
          <span class="button maximize"></span>
        </div>
        <div class="header-title">Terminal de Comandos</div>
      </div>
      <div class="terminal-background"></div>
      <div class="output" ref="outputRef">
        <div :id="line.replace(' -  ', '')" v-for="(line, index) in outputLines" :key="index">{{ line }}</div>
      </div>
      <div class="input-area">
        <span class="prompt">$ </span>
        <span class="input-text">{{ currentInput }}</span>
        <span class="cursor" :class="{ active: cursorActive }">|</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';

const outputLines = ref([]);
const currentInput = ref('');
const cursorActive = ref(true);
const outputRef = ref(null);
const commandIndex = ref(0);
const charIndex = ref(0);
const isUserInputEnabled = ref(false);
const commands = [
  'echo "Hola!👋 me llamo Carles"',
  'echo "Bienvenido a mi curriculum interactivo"',
  'echo "Soy un apasinado del diseño y desarrollo web 👨🏻‍💻"',
  'echo "Mi especialidad es el Front end"',
  'echo "Mis skills más potentes son Javascript & VUE"',
];


const typeCommand = () => {
  if (commandIndex.value < commands.length) {
    if (charIndex.value < commands[commandIndex.value].length) {
      currentInput.value += commands[commandIndex.value].charAt(charIndex.value);
      charIndex.value++;
      setTimeout(typeCommand, 100);
    } else {
      const fullCommand = commands[commandIndex.value];
      const finalText = fullCommand
        .slice(5)
        .replace(/^"|"$/g, '');
      outputLines.value.push(finalText);
      
      setTimeout(() => {
        currentInput.value = '';
        setTimeout(() => {
          commandIndex.value++;
          typeCommand();
        }, 500);
      }, 1500);
      
      charIndex.value = 0;
    }
  } else {
    outputLines.value.push('');
    outputLines.value.push('Terminal lista para recibir comandos. 🚀');
    outputLines.value.push('Escribe "help" para ver los comandos disponibles.');
    isUserInputEnabled.value = true;
  }
  scrollToBottom();
};

const processUserCommand = (input) => {
  switch (input.toLowerCase()) {
    case 'help':
      outputLines.value.push(' - 🚨 help');
      outputLines.value.push(' - 💼 experience');
      outputLines.value.push(' - 💻 projects');
      outputLines.value.push(' - 📄 download_cv');
      outputLines.value.push(' - 📩 contact');
      outputLines.value.push(' - ❌ clear');
      break;
    case 'experience':
      outputLines.value.push(
        ''
      );
      break;
    case 'projects':
      outputLines.value.push(
        ''
      );
      break;
    case 'contact':
      outputLines.value.push('');
      break;
    case 'download_cv':
      outputLines.value.push('');
      break;
    case 'clear':
      outputLines.value = [];
      break;
    default:
      outputLines.value.push(`Comando no reconocido: ${input}`);
  }
  scrollToBottom();
};

const handleKeyPress = (event) => {
  if (!isUserInputEnabled.value) return;

  if (event.key === 'Enter') {
    if (currentInput.value.trim() !== '') {
      outputLines.value.push(currentInput.value);
      processUserCommand(currentInput.value);
      currentInput.value = '';
    }
  } else if (event.key === 'Backspace') {
    currentInput.value = currentInput.value.slice(0, -1);
  } else if (event.key.length === 1) {
    currentInput.value += event.key;
  }
};

const blinkCursor = () => {
  cursorActive.value = !cursorActive.value;
  setTimeout(blinkCursor, 500);
};

const scrollToBottom = () => {
nextTick(() => {
  const outputEl = outputRef.value;
  if (outputEl) {
    outputEl.scrollTop = outputEl.scrollHeight;
  }
});
};

onMounted(() => {
  typeCommand();
  blinkCursor();
  window.addEventListener('keydown', handleKeyPress);
});

</script>

<style scoped>
.terminal-container {
  display: flex;
  justify-content: start;
  align-items: center;
  min-height: 100vh;
  position: relative;
  z-index: 999;
}

.terminal {
  position: relative;
  width: 600px;
  height: 425px;
  border-radius: 10px;
  color: #fff;
  padding: 0 0px;
  box-shadow:   
  0 0 10px rgba(255, 255, 255, 0.5),
    0 0 20px rgba(255, 255, 255, 0.4),
    0 0 30px rgba(255, 255, 255, 0.3);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  font-family: "Courier New", Courier, monospace;
  background-color: transparent;
  text-align: left;
}

.terminal-header {
  width: 100%;
  height: 30px;
  background-color: #fff;
  border-radius: 10px 10px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  box-sizing: border-box;
  z-index: 1;
}

.header-buttons {
  display: flex;
  gap: 8px;
}

.button {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
}

.close {
  background-color: #ff5f57;
}

.minimize {
  background-color: #ffbd2e;
}

.maximize {
  background-color: #28c940;
}

.header-title {
  flex: 1;
  text-align: center;
  font-weight: bold;
  color: #333;
}

.terminal-background {
  position: absolute;
  top: 25px;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  border-radius: 0 0 10px 10px;
  z-index: 0;
}

.output,
.input-area {
  position: relative;
  z-index: 1;
}

.output {
  flex: 1;
  overflow-y: auto;
  white-space: pre-wrap;
  padding-bottom: 10px;
  padding: 10px;
}

.input-area {
  display: flex;
  align-items: center;
  padding: 10px;
}

.prompt {
  color: #00ff00;
  margin-right: 5px;
}

.input-text {
  flex: 1;
  outline: none;
  white-space: nowrap;
  overflow: hidden;
  color: #00ff00;
}

.cursor {
  display: inline-block;
  width: 10px;
  background-color: #00ff00;
  margin-left: 2px;
}

.cursor.active {
  opacity: 1;
}

.cursor:not(.active) {
  opacity: 0;
}

@media (max-width: 768px) {
  .terminal {
    height: auto;
    width: 100%;
    max-width: 600px;
    height: 425px;
  }

  .terminal-header {
    height: 25px;
    font-size: 0.9rem;
  }

  .terminal-background{
    top: 20px;
  }

  .button {
    width: 10px;
    height: 10px;
  }

  .header-title {
    font-size: 0.8rem;
  }

  .output, .input-area {
    padding: 5px;
    font-size: 0.9rem;
  }

  .input-text {
    font-size: 0.9rem;
  }
}

@media (max-width: 480px) {
  .terminal {
    height: auto;
    width: 370px;
    max-width: 400px;
    height: 325px;
  }

  .terminal-header {
    height: 20px;
    font-size: 0.8rem;
  }
  .terminal-background{
    top: 20px;
  }
  .button {
    width: 8px;
    height: 8px;
  }

  .header-title {
    font-size: 0.7rem;
  }

  .output, .input-area {
    padding: 8px;
    font-size: 0.8rem;
  }

  .input-text {
    font-size: 0.8rem;
  }
}
</style>
