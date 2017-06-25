---
title: "Interface"
date: 2017-05-10

---

The birth of the interface make software development many convenient. We design some interfaces, and let others finish the corresponding modules. Thus, the application of the interface boosts the software scalability and maintainability. I will take computer hardware as an example.  

<p align="center">
  <img src="/images/post/20170510001.png" alt="Hardware interface"/>
</p>

Different CPU manufactured by different companies can be used on a same motherboard. It is because the manufacturer who produces the motherboard creates a public hardware interface for all CPU manufacturers so that CPU manafacturers can connect the same motherboard with their own CPU implementation.  


- - -

How to write an interface in c#? I will show you by the following case. For example, I want implement a media player. However, in real world, there are various of media files, such as mp3, wav and so on. In fact, there are some existed classes which allows us directly to implement the different file operation in c#. So I won't do it from scratch. But if we create the same numbers of the classes with the numbers of the type of the files, it will make things more complex. This is where the interface comes in.  
  
***  
  
#### **The media player interface:**

            namespace OscarPlayer
            {
                interface IPlayer
                {
                    void PlaySound(string url);
            
                    void PauseSound();
            
                    void ResumeSound();
            
                    void StopSound();
                }
            }
       
You can see that I just define some methods in the interface without any details, the interface will authorized the classes that will inherit itself to implement the specific methods.  
  
 *** 
  
#### **The mp3 player part:**  



        using WMPLib;
        namespace OscarPlayer
        {
            class MP3Player:IPlayer
            {
                private WMPLib.WindowsMediaPlayer _objPlayer;
        
                public MP3Player()
                {
                    _objPlayer = new WindowsMediaPlayerClass();
                }
        
        
                public void PlaySound(string url)
                {
                    _objPlayer.URL = url;
                    _objPlayer.controls.play();
                }
        
                public void PauseSound()
                {
                    if (_objPlayer != null)
                    {
                        _objPlayer.controls.pause();
                    }
                }
        
                public void ResumeSound()
                {
                    if (_objPlayer != null)
                    {
                        _objPlayer.controls.play();
                    }
                }
        
                public void StopSound()
                {
                    if (_objPlayer != null)
                    {
                        _objPlayer.controls.stop();
                    }
                    
                }
        
            }
        }  
        
***
  
#### **The wav player part:**  

        using System;
        using System.Media;
        
        namespace OscarPlayer
        {
            class WAVPlayer:IPlayer
            {
                private SoundPlayer _objPlayer;
        
                public WAVPlayer()
                {
                    _objPlayer = new SoundPlayer();
                }
        
        
                public void PlaySound(string url)
                {
                    _objPlayer.SoundLocation = url;
                    _objPlayer.Load();
                    _objPlayer.Play();  
        
                }
        
                public void PauseSound()
                {
                    if (_objPlayer != null)
                    {
                        
                    }
                }
        
                public void ResumeSound()
                {
                    throw new NotImplementedException();
                }
        
                public void StopSound()
                {
                    if (_objPlayer != null)
                    {
                        _objPlayer.Stop();    
                    }
                    
                }
            }
        }


So we can create different classes which have the same methods because of the inhert of the same interface. Although the inner implementation is totally different, we do not need to care. Next we will see the miracle of the interface.
  
***

#### **The declaration and use of the object**  

#### At first, I declare an interface object like a global variable.  

`private IPlayer _objPlayer = null;`  

#### Then, I use the file suffix to determine what kind of object do I need to create.  

        private void PlaySound(string path)
        {
            if (_objPlayer != null)
            {
                StopSound();
            }

            int pos = path.LastIndexOf(@".", StringComparison.Ordinal);
            string fileType = path.Substring(pos + 1);

            if (fileType.ToLower().Equals("wav"))
            {
                _objPlayer = new WAVPlayer();
            }
            else if (fileType.ToLower().Equals("mp3") || fileType.ToLower().Equals("wma"))
            {
                _objPlayer = new MP3Player();
                
            }
            _objPlayer.PlaySound(path);
            
        }
        
It is easy to use, right? The coolest thing is that it is quite convenient during the development. If some new types of the files are required, what you need to do is just finishing the corresponding class inheriting the `IPlayer` interface. That's all. Only one handle of the player is kept in this whole application any time.  

Noted that every time before coding, you need to think about how to make your program better, such as scalability, maintainability and feectiveness etc.



        



