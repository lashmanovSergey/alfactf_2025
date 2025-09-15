### Task "Retrowave Radio"

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
Are there any other ways to go to desired directory? Yes, we have /proc directories, that didn't restriced. Thus we can easily bypass this security check and get content of /copyright.

#### Exploitation
##### step 1 (navigation through /proc/... to /proc/.../copyright/<song_name>):
<img width="741" height="38" alt="image" src="https://github.com/user-attachments/assets/6a647299-a6bd-4a88-a1b2-bf36c44c4c43" />
and we got a desired music:
<img width="1367" height="639" alt="image" src="https://github.com/user-attachments/assets/35808c75-647d-4ddb-9eaf-59b3695a6f41" />

##### step 2 (listen music to get flag):
You just need to listen a first few words to understand flag :3

#### Congrats!!

