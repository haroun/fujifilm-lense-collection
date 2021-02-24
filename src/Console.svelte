<script>
  let output = []

  const log = (level, ...args) => {
    output = [[level, ...args.map(JSON.stringify)]].concat(output)
  }

  const inlineConsole = {
    log: (...args) => log('LOG', ...args),
    debug: (...args) => log('DEBUG', ...args),
    info: (...args) => log('INFO', ...args),
    warn: (...args) => log('WARN', ...args),
    error: (...args) => log('ERROR', ...args),
  }

  window.console.log = inlineConsole.log
  window.console.debug = inlineConsole.debug
  window.console.info = inlineConsole.info
  window.console.warn = inlineConsole.warn
  window.console.error = inlineConsole.error

  window.onerror = function(message, source, lineno, colno, error) {
    window.console.error(message, source, lineno, colno, error);
  };

  window.addEventListener('error', function(event) {
    window.console.error(event);
  });

  window.onrejectionhandled = function(event) {
    window.console.error(event.reason)
  }
  window.addEventListener('unhandledrejection', function(event) {
    window.console.warn(`UNHANDLED PROMISE REJECTION: ${event.reason}`);
  });
  window.onunhandledrejection = function(event) {
    window.console.error(event.reason)
  }
</script>

<pre>
	{output.join('\n')}
  {output.length}
</pre>

<style>
  pre {
    height: 10rem;
    background-color: #ddddddaa;
    border: 1px dotted #4e4e4e;
    position: fixed;
    bottom: 10em;
    width: 96%;
    text-align: left;
    overflow: auto;
  }
</style>
