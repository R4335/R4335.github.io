# The Universal Interface - Intro to the linux kernel - Part 2
### This is the most famous idealogy in linux - "Everything is a file"... But what it mean?

It means the kernel decided to simplify its job, instead of having one way to talk to a hard drive, another way to talk to a mouse and third way to talk to a network card, the kernel pretends everything is just a stream of bytes.
- Do you want to write a data to the hard drive? Write to a file.
- Do you want to play sound? Write to a file representing the speaker.
- Do you want to send data over the internet? Write to a file representing socket.

### Talking to Hardware
Lets look at some "files" that aren't files at all.

- The Black Hole: `/dev/null`, this is a device file that discards everything written to it. Its the void.
  ```
  echo "Hello void" > /dev/null
  ```
  Nothing happens. The kernel caught your data and deleted it.

- The Random Generator: `/dev/urandom`, this file generates infinite random garbage (noise). It is used for encryption.
  ```
  head -c 10 /dev/urandom | xxd
  ```
  ![/dev/random](https://raw.githubusercontent.com/R4335/R4335.github.io/main/blog/assets/urandom.webp)

  It didn't run a "random number generator program". It read the first 10 bytes of randomness and displayed as hex. The kernel did the hard work of collecting environmental noise like mouse movements, fan speed to generate entropy and served it as a simple file stream.

- Controlling Hardware: Keyboard LEDs
  Your keyboard LEDs (Caps Lock, Num Lock) aren't just lights; they are writeable files in the system directory.
  ```
  # move to the LED directory (path may vary slightly based on hardware)
  cd /sys/class/leds/input*::capslock/

  # turn ON caps lock light(without pressing the key)
  echo 1 | sudo tee brightness

  # same, to turn OFF that light
  echo 0 | sude tee brightness
  ```

  No need of C driver or a special keyboard utility. Just wrote the number 1 into a text file named *brightness*. The kernel saw that write operation and sent an electrical signal to your keyboard. This is how "sysfs"(/sys) works. It maps hardware controls to text files.

- The Raw Hard Drive: `/dev/sda` or `/dev/nvme0n1`
  Most people interact with files inside a partition (like EXT4). But the hard drive itself is just one giant file filled with bytes.
  ```
  sudo dd if=/dev/sda count=1 bs=512 | xxd
  # it read the first 512 bytes of the hard drive (master boot record)
  ```
  ![Raw Hard Drive](https://raw.githubusercontent.com/R4335/R4335.github.io/main/blog/assets/raw_hard_drive.webp)

  If you were to write random data to this "file" (`cat /dev/urandom > /dev/sda`) you would instantly wipe your entire operating system. The kernel doesn't stop you because, to it.. the hard drive is just a file waiting for data.


***Will Continue part 3 soon...***
