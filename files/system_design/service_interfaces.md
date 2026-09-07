# Service Interfaces

## Purpose

This doc locks the service contracts that the rest of the project must use.

The goal is to prevent AI agents from inventing incompatible save, auth, config, analytics, and session APIs in different parts of the codebase.

## Required Interfaces

Define these as explicit C# interfaces in the foundation layer:

```csharp
public interface ISaveService
{
    SaveResult LoadProfile(string profileId);
    SaveResult SaveProfile(string profileId, ProfileData data);
    SaveResult DeleteProfile(string profileId);
    SaveResult BackupProfile(string profileId);
    bool HasProfile(string profileId);
}

public interface IAuthService
{
    AuthState CurrentState { get; }
    Task<AuthResult> SignInGuestAsync();
    Task<AuthResult> SignInPlatformAsync();
    Task<AuthResult> RestoreSessionAsync();
    Task SignOutAsync();
}

public interface IRemoteConfigService
{
    Task<ConfigFetchResult> RefreshAsync();
    T GetValue<T>(string key, T fallbackValue);
    bool IsFeatureEnabled(string key, bool fallbackValue = false);
    DateTimeOffset? LastFetchTime { get; }
}

public interface IAnalyticsService
{
    void TrackEvent(string eventName, IReadOnlyDictionary<string, object> payload);
    void TrackScreen(string screenName, IReadOnlyDictionary<string, object>? payload = null);
    void TrackError(string errorCode, string message, IReadOnlyDictionary<string, object>? payload = null);
    Task FlushAsync();
}

public interface ISessionService
{
    SessionState CurrentState { get; }
    SessionContext CreateSession(SessionRequest request);
    void EndSession(SessionEndReason reason);
    void ResumeSession(SessionContext context);
}

public interface IContentService
{
    Task<ContentLoadResult> PreloadBootContentAsync();
    Task<ContentLoadResult> LoadModeContentAsync(string modeId);
    bool IsContentReady(string contentKey);
}
```

## Contract Rules

1. Services are interfaces first, concrete classes second.
2. UI must depend on the interface, not the implementation.
3. Gameplay code must not call Unity services directly if a project service exists.
4. Services should be injectable through a bootstrapper or service locator used only at the composition root.
5. The interface surface should remain stable across feature branches.

## Expected Behavior

- `ISaveService` owns persistence behavior.
- `IAuthService` owns identity state and login restoration.
- `IRemoteConfigService` owns tunable runtime values.
- `IAnalyticsService` owns telemetry.
- `ISessionService` owns app/session lifecycle.
- `IContentService` owns preload and on-demand content readiness.

## Dependencies

- Unity foundation and bootstrap code
- Feature 01 data contracts
- save schema versioning doc

## Tests

- implementations can be swapped without changing consumers
- service interfaces compile in isolation
- session and save contracts do not depend on scene objects

## Exit Criteria

- every major system uses these interfaces
- no feature defines its own duplicate service contract
- the codebase has one obvious place to extend each service

