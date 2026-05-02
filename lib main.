import 'package:flutter/material.dart';
import 'package:on_audio_query/on_audio_query.dart';
import 'package:just_audio/just_audio.dart';
import 'package:permission_handler/permission_handler.dart';

void main() {
  runApp(const MaterialApp(
    debugShowCheckedModeBanner: false,
    home: LGPlayer(),
  ));
}

class LGPlayer extends StatefulWidget {
  const LGPlayer({super.key});

  @override
  State<LGPlayer> createState() => _LGPlayerState();
}

class _LGPlayerState extends State<LGPlayer> {
  final OnAudioQuery _audioQuery = OnAudioQuery();
  final AudioPlayer _audioPlayer = AudioPlayer();

  @override
  void initState() {
    super.initState();
    requestPermission();
  }

  void requestPermission() async {
    // اسٹوریج کی اجازت لینا تاکہ گانے نظر آئیں
    await Permission.storage.request();
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.black,
      appBar: AppBar(
        title: const Text("LG PLAYER", style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
        backgroundColor: Colors.redAccent,
        centerTitle: true,
      ),
      body: FutureBuilder<List<SongModel>>(
        future: _audioQuery.querySongs(
          sortType: null,
          orderType: OrderType.ASC_OR_SMALLER,
          uriType: UriType.EXTERNAL,
          ignoreCase: true,
        ),
        builder: (context, item) {
          if (item.data == null) return const Center(child: CircularProgressIndicator());
          if (item.data!.isEmpty) return const Center(child: Text("No Songs Found", style: TextStyle(color: Colors.white)));

          return ListView.builder(
            itemCount: item.data!.length,
            itemBuilder: (context, index) => ListTile(
              title: Text(item.data![index].displayNameWOExt, style: const TextStyle(color: Colors.white)),
              subtitle: Text("${item.data![index].artist}", style: const TextStyle(color: Colors.grey)),
              leading: const Icon(Icons.music_note, color: Colors.redAccent),
              onTap: () async {
                try {
                  await _audioPlayer.setAudioSource(AudioSource.uri(Uri.parse(item.data![index].uri!)));
                  _audioPlayer.play();
                } catch (e) {
                  print("Error playing audio: $e");
                }
              },
            ),
          );
        },
      ),
    );
  }
}
