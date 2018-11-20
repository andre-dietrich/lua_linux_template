<!--
author:   Your Name

email:    your@mail.org

version:  0.0.1

language: en

narrator: US English Female

script:     js/libv86.js


@eval
<script>
// https://gist.github.com/creationix/2502704
// Implement bash string escaping.
function bashEscape(arg) {
    return "'" + arg.replace(/'+/g, function (val) {
        return "'" + val.replace(/'/g, "\\'") + "'";
        }) + "'";
}

let input = "lua -e " + bashEscape(`@input`) + "\n";
let data  = "";

function resetSerial(container = null) {
    function char_listener(char){
        if(char !== "\r")
            data += char;
        if(data.endsWith("login: "))
            window.emulator.serial0_send("root\n");
        else if(data.endsWith("/root% "))
        {
            if(container)
                container.hidden = true;

            if(input) {
                window.emulator.serial0_send(input);
                input = null;
            } else {
                send.lia("eval", "LIA: stop");
                window.running = false;
            }
        }
    };

    function line_listener(line){
        // filter noise
        if(!line.startsWith("/root% lua -e") &&
           !line.startsWith("> ") &&
           line.indexOf("Welcome to Buildroot") === -1 &&
           line.indexOf("login:") === -1 &&
           line.trim() !== "")
        {
            send.lia("output", line);
        }
    };

    if(window.listener) {
        window.emulator.remove_listener("serial0-output-char", window.listener[0]);
        window.emulator.remove_listener("serial0-output-line", window.listener[1]);
    }

    window.emulator.add_listener("serial0-output-char", char_listener);
    window.emulator.add_listener("serial0-output-line", line_listener);

    window.listener = [char_listener, line_listener];
}



if(!window.emulator) {
    window.running = true;

    let container = document.getElementById("screen_container@0");

    if(container)
        container.hidden=false;

    window.emulator = new V86Starter({
        memory_size: 32 * 1024 * 1024,
        vga_memory_size: 2 * 1024 * 1024,
        // Uncomment to see what's going on
        screen_container: document.getElementById("screen_container@0"),
        bios: { url: "http://127.0.0.1:4892/seabios.bin" },
        vga_bios: { url: "http://127.0.0.1:4892/vgabios.bin" },
        cdrom: { url: "http://127.0.0.1:4892/linux26.iso" },
        autostart: true,
        disable_keyboard: false,
    });

    resetSerial(container);

    "LIA: wait";

} else if(!window.running) {
    window.running = true;
    resetSerial();
    window.emulator.serial0_send(input);
    input = null;

    "LIA: wait";

} else {

    "Waiting for another process to finish!";

}
</script>


<span id="screen_container@0" hidden="true">
  <div class="lia-code-stdout"></div>
  <canvas style="display: none"></canvas>
</span>
@end

-->

# Course Main Title


``` lua
k = 1
x = 0

while k < 1000 do
    x = x + 1 / (k * k)
    k = k + 2
end

print(math.sqrt(x*8))

function factorial(n)
    if n == 0 then
        return 1
    else
    return n * factorial(n - 1)
    end
end

print("factorial(10):", factorial(10))
```
@eval(fak1)





``` lua
k = 1
x = 0

while k < 1000 do
    x = x + 1 / (k * k)
    k = k + 2
end

print(math.sqrt(x*8))

function factorial(n)
    if n == 0 then
        return 1
    else
    return n * factorial(n - 1)
    end
end

print("factorial(10):", factorial(10))
```
@eval(fak2)
