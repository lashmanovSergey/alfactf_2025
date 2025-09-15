<img width="1400" height="559" alt="image" src="https://github.com/user-attachments/assets/cdc8933f-6a82-4a36-8c1f-aaf98482a9f7" />### Task "Retrowave Radio"

Difficulty: Easy

Category: Web

#### Analysis

Our goal is to listen restricted music. We are given with source code. Lets examine it:
```python
@app.route('/api/songs')
def api_songs():
    """API endpoint to get list of songs"""
    songs = get_songs_list()
    return jsonify(songs)
```

This fragment of code says us that if we locate /api/songs we get all songs. Lets check this endpoint:
<img width="585" height="140" alt="image" src="https://github.com/user-attachments/assets/733ef859-3354-4bfa-ae2c-bc629e23cc50" />

Now we understand, that the wished music is located in /copyright directory. The navigation throught file system is performed by the following code:
```python
@app.route('/<path:filename>')
def serve_song(filename):
    filename = os.path.normpath("/"+filename)
    if Path(filename).expanduser().absolute().is_rel ative_to("/copyright"):
        return filename, 403
    return send_file(filename)
```
Here we see path normalization. It performs dots operations, like "/etc/.." to "/" directory. And because of it we cannot easily bypass is_relative_to function.
Are there any other ways to go to desired directory? Yes, we have /proc directory, that isn't restricted. Thus we can easily bypass the above security check and get content of /copyright.

#### Exploitation
##### step 1 (navigation through /proc/... to /proc/.../copyright/<song_name>):
<img width="1400" height="559" alt="image" src="https://github.com/user-attachments/assets/a1f2fd68-b12c-43c5-a697-702c80407af9" />

##### step 2 (listen music to get flag):
You just need to listen a first few words to understand flag :3

#### Congrats!!

