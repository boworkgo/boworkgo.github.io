---
layout: splash
title: "PlaidCTF 2019 Writeup"
classes:
  - landing
  - dark-theme
  - wide
---

# 4-15-2019 12:10 AM

### Docker

Just so you know, or I might be wrong: a docker image is the immutable source code, and docker container is an image's running instance. I pulled the challenge image and tried to grep for the flag in the container. When that didn't work, I searched for the flag in the image location.

*solution.sh*

    sudo docker pull whowouldeverguessthis/public
    sudo docker images
    sudo docker image inspect whowouldeverguessthis/public
    sudo docker ps -a
    sudo docker run -it --name testPCTF whowouldeverguessthis/public bash
    	grep -rI --exclude-dir=proc --exclude-dir=sys 'PCTF{' .
    cd /var/lib/docker/
    sudo grep -rI 'PCTF{' .

Flag: `PCTF{well_it_isnt_many_points_what_did_you_expect}`

### Everland

This was a game hacking problem. First, I guessed that recuperating every turn would let me defeat all the monsters except for the Possessed Monster. Then, I found a function in the game called `Sacrifice`. I found the health of the Possessed monster was defined by `max((!strength)*5, 250)`, so I needed to alter the former parameter. With some more guessing, I altered the first parameter to be `65 * 5 = 325`, enabling the `Sacrifice` function, enabling me to defeat the Possessed monster.

*everland.sml* (Original file)
	
	...
    if (!should_capture) andalso (not (!has_captured)) 
    then
      if (!e_h > 50) then
    (TextIO.print ("It was too strong, you failed to capture "^
      (color e_name ORANGE));
    enemy)
      else
      let
    val _ = should_capture := false
    val _ = has_captured := true
    val _ = captured := enemy
    (* Kill them so that you can heal yourself *)
    fun sacrifice_fn (my_h, my_s, their_h, their_s) =
      (fn () => (
     my_h := min((!my_h)+min(!e_h, !my_s*10), player_max);
     e_h  := (!e_h-(!my_h)*10);
     p_ms := List.filter (fn (n, _) => n <> "Sacrifice") (!p_ms)),
       fn () => ()) (* Only used by the AI, not us *)
    val _ = p_ms := (List.filter (fn (n, _) => n <> "Capture") (!p_ms))
    @[("Sacrifice", sacrifice_fn)]
	...


*solution.py*

    from pwn import *
    
    r = remote('everland.pwni.ng', '7772')
    r.sendline('Greg')
    for x in range(79):
    r.sendline('fight')
    r.sendline('2')
    for x in range(5):
    r.sendline('forage')
    r.sendline('use')
    r.sendline('2')
    r.sendline('use')
    r.sendline('1')
    r.sendline('use')
    r.sendline('3')
    r.sendline('fight')
    r.sendline('4')
    r.sendline('use')
    r.sendline('1')
    r.sendline('fight')
    r.sendline('5')
    r.sendline('fight')
    r.sendline('5')
    r.interactive()

Flag: `PCTF{just_be_glad_i_didnt_arm_cpt_hook_with_GADTs}`