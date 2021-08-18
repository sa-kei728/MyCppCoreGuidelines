# Interfaces
インターフェイスとはプログラムの2つの部分の間の契約のことです.  
あるサービスの供給者とそのサービスの利用者に何を期待するかを正確に記述することが重要です.  
優れた(わかりやすい, 効率的な使用を促す, エラーを起こさない, テストをサポートするなど)インターフェイスを持つことは,  
おそらくコードの構成において最も重要なことです.  

[I.1: Make interfaces explicit](./01_MakeInterfacesExplicit/README.md)  
[I.2: Avoid non-const global variables](./02_AvoidNonConst/README.md)  
[I.3: Avoid singletons](./03_AvoidSingletons/README.md)  
[I.4: Make interfaces precisely and strongly typed](./04_MakeInterfacesPreciselyAndStronglyTyped/README.md)  
[I.5: State preconditions (if any)](./05_StatePreconditions/README.md)  
[I.6: Prefer Expects() for expressing preconditions](./06_PreferExpects/README.md)  
[I.7: State postconditions](./07_StatePostconditions/README.md)  
[I.8: Prefer Ensures() for expressing postconditions](./08_PreferEnsures/README.md)  
[I.9: If an interface is a template, document its parameters using concepts](./09_InterfaceIsTemplateDocumentParameters/README.md)  
[I.10: Use exceptions to signal a failure to perform a required task](./10_UseExceptionToSignalAFailure/README.md)  
[I.11: Never transfer ownership by a raw pointer (T*) or reference (T&)](./11_NeverTransferOwnershipByARawPointer/README.md)  
[I.12: Declare a pointer that must not be null as not_null](./12_DeclareAPointerMustntBeNullAsNotNull/README.md)  
[I.13: Do not pass an array as a single pointer](./13_DoNotPassAnArrayAsASinglePointer/README.md)  
[I.22: Avoid complex initialization of global objects](./22_AvoidComplexInitialization/README.md)  
[I.23: Keep the number of function arguments low](./23_KeepTheNumberOfFunction/README.md)  
[I.24: Avoid adjacent parameters that can be invoked by the same arguments in either order with different meaning](./24_AvoidAdjacentParameters/README.md)  
[I.25: Prefer empty abstract classes as interfaces to class hierarchies](./25_PreferEmptyAbstractClassAsInterfaces/README.md)  
[I.26: If you want a cross-compiler ABI, use a C-style subset](./26_IfWantCrossCompilerUseCStyleSubset/README.md)  
[I.27: For stable library ABI, consider the Pimpl idiom](./27_ForStableLibraryConsiderPimplIdiom/README.md)  
[I.30: Encapsulate rule violations](./30_EncapsulateRuleViolations/README.md)  
