

### 修正说明
根目录下的`CMakeLists.txt` 中需要增加`INCLUDE(GNUInstallDirs)` 才能让`CMAKE_INSTALL_FULL_*`获得值，从而让install命令正常运行，
然而将本库用于eosio却不需要添加上述语句，因为eosio根目录的CMakeLists.txt已经添加了上述include语句。
根目录下的`version.cmake.in`中用到了`describe --tags --dirty`，这条命令需要本repo有tag标志，因此需要打tag，才能运行通过，
然而将本库用于eosio却不需要添加上述语句，因为eosio已经有tag。


### 代码结构 - 最简单
``` 


```


### 代码结构 - 详细
``` text


plugin.hpp
    namespace appbase{
        class application;
        application& app();
    
        class abstract_plugin {
            public:
                enum state {
                    registered, ///< the plugin is constructed but doesn't do anything
                    initialized, ///< the plugin has initialized any state required but is idle
                    started, ///< the plugin is actively running
                    stopped ///< the plugin is no longer running
                };
    
                virtual ~abstract_plugin(){}
                virtual state get_state()const = 0;
                virtual const std::string& name()const  = 0;
                virtual void set_program_options( options_description& cli, options_description& cfg ) = 0;
                virtual void initialize(const variables_map& options) = 0;
                virtual void startup() = 0;
                virtual void shutdown() = 0;
        };
    
        template<typename Impl>
        class plugin;
    }

channel.hpp
    namespace appbase{
        <anonymous> : void *
        drop_exceptions
            drop_exceptions()
            operator()(InputIterator, InputIterator) : result_type
        channel
            handle
                ~handle() : void
                unsubscribe() : void
                handle()
                handle(handle &&)
                operator=(handle &&) : handle &
                handle(const handle &)
                operator=(const handle &) : handle &
                _handle : handle_type
                handle(handle_type &&)
                channel
            publish(const Data &) : void
            subscribe(Callback) : handle
            set_dispatcher(const DispatchPolicy &) : enable_if_t<value, void>
            has_subscribers() : bool
            channel(const ios_ptr_type &)
            ~channel() : void
            deleter(void *) : void
            get_channel(erased_channel_ptr &) : channel *
            make_unique(const ios_ptr_type &) : erased_channel_ptr
            ios_ptr : ios_ptr_type
            <anonymous> : const Data &
            _signal : signal<void(const Data &), DispatchPolicy>
            appbase::application

        channel_decl
        is_channel_decl_impl(const channel_decl<Ts...> *) : true_type
        is_channel_decl_impl(...) : false_type
    }

method.hpp
    namespace appbase{
        <anonymous> : void *
        first_success_policy
        first_success_policy
        first_success_policy
        first_provider_policy
        first_provider_policy
        first_provider_policy
        namespace impl {
            method_caller
            method_caller
            method_caller
        }
        method
            handle
                ~handle() : void
                unregister() : void
                handle()
                handle(handle &&)
                operator=(handle &&) : handle &
                handle(const handle &)
                operator=(const handle &) : handle &
                _handle : handle_type
                handle(handle_type &&)
                method

            register_provider(T, int) : handle
            method()
            ~method() : void
            deleter(void *) : void
            get_method(erased_method_ptr &) : method *
            make_unique() : erased_method_ptr
            appbase::application

        method_decl
        is_method_decl_impl(const method_decl<Tag, FunctionSig, DispatchPolicy> *) : true_type
        is_method_decl_impl(...) : false_type
    }

application.hpp : plugin.hpp,channel.hpp,method.hpp
    namespace appbase{
        class application{
            ~application() : void
            set_version(uint64_t) : void
            version() const : uint64_t
            version_string() const : string
            set_default_data_dir(const path &) : void
            data_dir() const : path
            set_default_config_dir(const path &) : void
            config_dir() const : path
            get_logging_conf() const : path
            initialize(int, char * *) : bool
            startup() : void
            shutdown() : void
            exec() : void
            quit() : void
            instance() : application &
            find_plugin(const string &) const : abstract_plugin *
            get_plugin(const string &) const : abstract_plugin &
            register_plugin() : auto &
            find_plugin() const : Plugin *
            get_plugin() const : Plugin &
            get_method() : enable_if_t<value, method_type &>
            get_channel() : enable_if_t<value, channel_type &>
            get_io_service() : io_service &
            plugin
            initialize_impl(int, char * *, vector<abstract_plugin *>) : bool
            plugin_initialized(abstract_plugin &) : void
            plugin_started(abstract_plugin &) : void
            application()
            plugins : map<string, unique_ptr<abstract_plugin>>
            initialized_plugins : vector<abstract_plugin *>
            running_plugins : vector<abstract_plugin *>
            methods : map<type_index, erased_method_ptr>
            channels : map<type_index, erased_channel_ptr>
            io_serv : shared_ptr<io_service>
            set_program_options() : void
            write_default_config(const path &) : void
            print_default_config(ostream &) : void
            application_impl
            my : unique_ptr<struct application_impl>
        }
        
        application& app();
        
        template<typename Impl>
        class plugin : public abstract_plugin{
            plugin()
            ~plugin() : void
            get_state() const : state
            name() const : const string &
            register_dependencies() : void
            initialize(const variables_map &) : void
            startup() : void
            shutdown() : void
            plugin(const string &)
            _state : state
            _name : string
        }
    }


version.hpp


application.cpp



```