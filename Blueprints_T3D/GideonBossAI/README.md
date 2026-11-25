# Gideon Boss AI (vs Greystone) - Unreal Engine 4.27

Este directorio contiene los nodos **T3D** listos para copiar y pegar (`Ctrl+C` -> `Ctrl+V`) en Unreal Engine 4.27 para convertir a **GideonPlayerCharacter** en un Boss que persigue y ataca al jugador (**Greystone.uasset**).

## Assets a crear en `Content/ParagonGideon/AI/`
1. **`BB_GideonBoss`** (*Artificial Intelligence -> Blackboard*)
   - `TargetActor` (Object -> Base Class: Actor)
   - `DistanceToTarget` (Float)
   - `bInAttackRange` (Bool)
   - `AttackCount` (Int)
2. **`AIC_GideonBoss`** (*Blueprint Class -> AIController*) -> pegar contenido de `AIC_GideonBoss.t3d`.
3. **`BTS_UpdateGreystone`** (*Blueprint Class -> BTService_BlueprintBase*) -> pegar contenido de `BTS_UpdateGreystone.t3d`.
4. **`BTT_GideonComboAttack`** (*Blueprint Class -> BTTask_BlueprintBase*) -> pegar contenido de `BTT_GideonComboAttack.t3d`.
5. **`BT_GideonBoss`** (*Artificial Intelligence -> Behavior Tree*, Blackboard = `BB_GideonBoss`):
   - `Root` -> `Selector` (Service: `BTS_UpdateGreystone`)
     - **Rama 1 (Atacar):** `Sequence` (Decorator Blackboard: `bInAttackRange` Is Set) -> `BTT_GideonComboAttack` -> `Wait (1.2s)`
     - **Rama 2 (Perseguir):** `Sequence` (Decorator Blackboard: `TargetActor` Is Set) -> `Move To` (`TargetActor`, `Acceptable Radius = 180`)
6. En **`GideonPlayerCharacter.uasset`** (Pawn): asignar `AI Controller Class = AIC_GideonBoss` y `Auto Possess AI = Placed in World or Spawned`.
7. Colocar un **`NavMeshBoundsVolume`** en el nivel cubriendo el suelo de combate (pulsar `P` para comprobar el NavMesh verde).

*Alternativa sin Behavior Tree:* pegar `Gideon_BossAI_EventGraph.t3d` directamente en el EventGraph de `GideonPlayerCharacter`.