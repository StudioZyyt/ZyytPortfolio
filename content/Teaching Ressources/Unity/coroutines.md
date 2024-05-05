---
title: Coroutines
draft: true
tags:
  - unity
---
# Introduction

A coroutine allows you to spread tasks across several frames. In Unity, a coroutine is a method that can pause execution and return control to Unity but then continue where it left off on the following frame.

In most situations, when you call a method, it runs to completion and then returns control to the calling method, plus any optional return values. This means that any action that takes place within a method must happen within a single frame update.

# Examples

```csharp
private IEnumerator ExecuteEveryFrame() 
{
	Color32 color;
		for (int r = 0; r < 255; r++) {
			color.r = r;
			renderer.color = color;
			yield return null;
		}
}
```

```csharp
private IEnumerator ExecuteAfterSeconds(int seconds) 
{
	yield return new WaitForSeconds(seconds);
	Execute();
}
```

```csharp
private IEnumerator DestroyAfterAudio(AudioClip clip) 
{
  AudioSource source = GetComponent<AudioSource>().PlayOneShot(clip);
  while (source.isPlaying)
		yield return null;
	Destroy(gameObject);
}
```

```csharp
private IEnumerator ExecuteOnceAvailable() 
{
	GameManager gameManager = null;
	while ((gameManager = FindObjectOfType<GameManager>()) == null) 
		yield return null;
	
	gameManager.Execute();
}
```

[Example](https://www.youtube.com/watch?v=7RBI9mb8s3E) of a clean way to check if coroutines have finished.