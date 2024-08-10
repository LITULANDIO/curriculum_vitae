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

<script>
import { ref, onMounted, nextTick } from 'vue';

export default {
  name: 'Terminal',
  setup() {
    const outputLines = ref([]);
    const currentInput = ref('');
    const cursorActive = ref(true);
    const outputRef = ref(null);


    const commands = [
      'echo "Hola!👋 me llamo Carles"',
      'echo "Bienvenido a mi curriculum interactivo"',
      'echo "Soy un apasinado del diseño y desarrollo web 👨🏻‍💻"',
      'echo "Mi especialidad es el Front end"',
      'echo "Mis skills más potentes son Javascript & VUE"',
    ];

    const commandIndex = ref(0);
    const charIndex = ref(0);
    const isUserInputEnabled = ref(false);

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
            'QUE MARAVILLA EL CHORRI'
          );
          break;
        case 'projects':
          outputLines.value.push(
            'verdes, rojas, amarillas, que buenas que ricas'
          );
          break;
        case 'contact':
          outputLines.value.push('una rallica per fer la gracia');
          break;
        case 'download_cv':
          outputLines.value.push('una rallica per fer la gracia');
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

    return {
      outputLines,
      currentInput,
      cursorActive,
    };
  },
};
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
  padding: 0 0px; /* El padding se maneja en la cabecera */
  box-shadow:   
  0 0 10px rgba(255, 255, 255, 0.5), /* Lado izquierdo */
    0 0 20px rgba(255, 255, 255, 0.4), /* Lado derecho */
    0 0 30px rgba(255, 255, 255, 0.3); /* Parte inferior */
  display: flex;
  flex-direction: column;
  overflow: hidden; /* Oculta el desbordamiento */
  font-family: "Courier New", Courier, monospace;
  background-color: transparent;
  text-align: left;
}

.terminal-header {
  width: 100%;
  height: 30px; /* Altura del marco superior */
  background-color: #fff; /* Color de fondo blanco para el marco superior */
  border-radius: 10px 10px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  box-sizing: border-box;
  z-index: 1; /* Pon el marco superior delante del fondo */
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
  color: #333; /* Texto negro para el marco superior */
}

.terminal-background {
  position: absolute;
  top: 25px; /* Empuja el fondo hacia abajo para que el marco superior quede visible */
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8); /* Fondo con opacidad */
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
  padding: 10px; /* Agrega un poco de espacio en la parte superior para la salida */
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

/* Ajustes específicos para pantallas pequeñas */
@media (max-width: 768px) {
  .terminal {
    height: auto; /* Permite que el terminal se ajuste a la altura del contenido */
    width: 100%;
    max-width: 600px; /* Máximo ancho para mantener un tamaño adecuado */
    height: 425px; /* Mantiene la altura fija en dispositivos móviles */
  }

  .terminal-header {
    height: 25px; /* Ajusta el tamaño del encabezado en pantallas pequeñas */
    font-size: 0.9rem; /* Reduce el tamaño de la fuente del título del encabezado */
  }

  .terminal-background{
    top: 20px;
  }

  .button {
    width: 10px;
    height: 10px;
  }

  .header-title {
    font-size: 0.8rem; /* Reduce el tamaño del texto del título */
  }

  .output, .input-area {
    padding: 5px; /* Reduce el padding en pantallas pequeñas */
    font-size: 0.9rem; /* Ajusta el tamaño de la fuente del input */
  }

  .input-text {
    font-size: 0.9rem; /* Ajusta el tamaño de la fuente del input */
  }
}

@media (max-width: 480px) {
  .terminal {
    height: auto; /* Permite que el terminal se ajuste a la altura del contenido */
    width: 370px;
    max-width: 400px; /* Máximo ancho para mantener un tamaño adecuado */
    height: 325px; /* Mantiene la altura fija en dispositivos móviles más pequeños */
  }

  .terminal-header {
    height: 20px; /* Ajusta aún más el tamaño del encabezado en pantallas muy pequeñas */
    font-size: 0.8rem; /* Ajusta el tamaño de la fuente del título del encabezado */
  }
  .terminal-background{
    top: 20px;
  }
  .button {
    width: 8px;
    height: 8px;
  }

  .header-title {
    font-size: 0.7rem; /* Ajusta el tamaño del texto del título */
  }

  .output, .input-area {
    padding: 8px; /* Reduce aún más el padding en pantallas pequeñas */
    font-size: 0.8rem; /* Ajusta el tamaño de la fuente del input */
  }

  .input-text {
    font-size: 0.8rem; /* Ajusta el tamaño de la fuente del input */
  }
}
</style>
