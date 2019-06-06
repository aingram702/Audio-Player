# Audio-Player
Python Audio Player
from tkinter import * # Import tkinter
import pyglet
global PATH
class ControlAudio:
    def __init__(self):
        window = Tk() # Create a window
        window.title("Python Audio Player") # Set a title
        
        self.width = 1000 # Width of the self.canvas
        self.canvas = Canvas(window, bg = "gray", 
            width = self.width, height = 200)
        self.canvas.pack()
        
        frame = Frame(window)
        frame.pack()
        btStop = Button(frame, text = "Stop", command = self.stop)
        btStop.pack(side = LEFT)
        btPlay = Button(frame, text = "Play", 
            command = self.play)
        btPlay.pack(side = LEFT)
        btForward = Button(frame, text = "Forward", 
            command = self.forward)
        btForward.pack(side = LEFT)
        btBackward = Button(frame, text = "Backward", 
            command = self.backward)
        btBackward.pack(side = LEFT)
        
        self.x = 0 # Starting x position
        self.sleepTime = 100 # Set a sleep time 
        self.canvas.create_text(self.x, 30, 
            text = "Message moving?", tags = "text")
        
        self.dx = 3
        self.isStopped = False
        self.playAudio()
        
        window.mainloop() # Create an event loop
        
    def stop(self): # Stop animation
        self.isStopped = True
    
    def play(self): # Resume animation
        self.isStopped = False   
        self.animate()
    
    def forward(self): # Speed up the animation
        if self.sleepTime > 5:
            self.sleepTime -= 20
               
    def backward(self): # Slow down the animation
        self.sleepTime += 20

#------------------------------------------------------------------------------------------------------------#        
    def animate(self): # Move the message
        while not self.isStopped:
            self.canvas.move("text", self.dx, 0) # Move text 
            self.canvas.after(self.sleepTime) # Sleep 
            self.canvas.update() # Update self.canvas
            if self.x < self.width:
                self.x += self.dx  # Set new position 
            else:
                self.x = 0 # Reset string position to the beginning
                self.canvas.delete("text") 
                # Redraw text at the beginning
                self.canvas.create_text(self.x, 30, 
                    text = "Message moving?", tags = "text")    
#-----------------------------------------------------------------------------------------------------------#

class mediaplayer:

    import pyglet     # import pyglet
    import datetime   
    import os
    import time
    import threading
    import pyglet.media as media

    # player
    jump_distance=30
    path = 'File Path Goes Here'
    song_time = ''
    song_duration = ''
    volume = ''
    player = ''
    self = ''

    
    def __init__(self, path, song_time,song_duration,volume):
        self.path=path                      # Song Playing Song
        self.volume=volume                  # Song Volume Update
        self.songtime=song_time             # Song Time Variable
        self.songduration=song_duration     # Song Duration
        self.player=media.Player()          # pyglet Media Player
        self.player.volume=1.5              # 
        self.time_thread()                  # Time Updating Thread

        self.path.trace('w',self.play_song)
        self.volume.trace('w', self.volume_)
        return path        
    def jump(self, time):
        try:
            self.player.seek(time)
            return 
        except:
            print ('[+] Jump is Not Possible')
            return
        
        
    def now(self):
        storeobj=self.player.time
        return storeobj
    
    def now_(self):
        time=int(self.now())
        k=datetime.timedelta(seconds=time)
        k=k.__str__()
        return k

        
    def pause(self):
        self.player.pause()    
        return 

    def play(self):
        self.player.play()
        return
    
    def stop(self):
        self.reset_player()
        return
    
    def volume_(self, *args, **kwargs):
        try:
            volume=self.volume.get()
            self.player.volume=volume
        except:
            pass
        return
    
    def time_thread(self):
        threading.Thread(target=self.update_time_).start()
        return
    
        
    def update_time_(self):
        while True:
            now=self.now_()
            try:
                self.songtime.set(now)
                pass
            
            except Exception as e:
                print(e)
                pass
        
    
    def duration(self):
        try:
            storeobj=self.player.source.duration
            return storeobj
        except:
            return '0'
    def duration_(self):
        time=self.duration()+10.0
        k=datetime.timedelta(seconds=time)
        k=k.__str__()
        return k
    
    def reset_player(self):
        self.player.pause()
        self.player.delete()
        return
            
            
    
    def play_song(self, *args, **kwargs):
        if self.path.get():
            try:
                self.reset_player()
                try:
                    src=media.load(self.path.get())
                    self.player.queue(src)
                    self.play()
                    
                    self.songduration.set(self.duration_())   # Updating duration Time
                    return 
                except Exception as e:
                    print ("[+] Something wrong when playing song",e)
                    return 
            except Exception as e:
                print (' [+] Please Check Your File Path', self.path.get())
                print (' [+] Error: Problem On Playing \n ',e)
                return 
        else:
            print (' [+] Please Check Your File Path', self.path.get())
        return

    def fast_forward(self):
        time = self.player.time + jump_distance
        try:
            if self.duration() > time:
                self.player.seek(time)
            else:
                self.player.seek(self.duration())
        except AttributeError:
            pass

    def rewind(self):
        time = self.player.time - jump_distance
        try:
            self.player.seek(time)
        except:
            self.player.seek(0)



# play song
PATH = 'Your File Path Goes Here'
mediaplayer.play_song(PATH)                                      
ControlAudio() # show GUI
